If you want users to get assigned a custom claim role to give them access to certain data when subscribed to a specific product, you can set a firebaseRole metadata value on the Stripe product (see screenshot).

The value you set for firebaseRole (e.g. “premium” in the screenshot above) will be set as a custom claim stripeRole on the user. This allows you to set specific security access rules based on the user’s roles, or limit access to certain pages. For example if you have one basic role and one premium role you could add the following to your Cloud Firestore rules:
```node
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    function hasBasicSubs() {
      return request.auth.token.stripeRole == "basic";
    }

    function hasPremiumSubs() {
      return request.auth.token.stripeRole == "premium";
    }

    match /content-basic/{doc} {
      allow read: if hasBasicSubs() || hasPremiumSubs();
    }
    match /content-premium/{doc} {
      allow read: if hasPremiumSubs();
    }
  }
}
```

Alternatively you can validate their role client-side with the JavaScript SDK. When doing so you need to make sure to force-refresh the user token:
```jsx
import { getAuth, getIdTokenResult } from "firebase/auth";

async function getCustomClaimRole() {
  const auth = getAuth();
  const user = auth.currentUser;

  if (user) {
    await user.getIdToken(true); // Force refresh if necessary
    const tokenResult = await getIdTokenResult(auth);
    return tokenResult.claims.stripeRole;
  } else {
    throw new Error("No user is signed in");
  }
}
```
## List available products and prices
Products and pricing information are normal collections and docs in your Cloud Firestore and can be queried as such:

```jsx
import { collection, getDocs, getFirestore } from "firebase/firestore";

const db = getFirestore();

async function fetchActiveProductsAndPrices() {
  const querySnapshot = await getDocs(collection(db, "products"), {
    where: { active: true },
  });

  querySnapshot.forEach(async (doc) => {
    console.log(doc.id, " => ", doc.data());

    const priceSnap = await getDocs(collection(doc.ref, "prices"));
    priceSnap.forEach((priceDoc) => {
      console.log(priceDoc.id, " => ", priceDoc.data());
    });
  });
}
```
> 1. **Imports:** Import specific functions from firebase/firestore.
> 2. **Accessing Firestore:** Use getFirestore() to retrieve the Firestore instance.
> 3. **Querying Collections:** Use collection(db, "products") to access the "products" collection.
> 4. **Filtering Data:** Pass a where clause to getDocs to filter documents.
> 5. **Retrieving Documents:** Use getDocs to fetch documents matching the query.
> 6. **Retrieving Subcollections:** Use collection(doc.ref, "prices") to access the "prices" subcollection.
> 7. **Iterating Over Documents:** Use forEach to loop through documents and subcollection documents.



## One-time payments on the web
You can create Checkout Sessions for one-time payments when referencing a one-time price ID. One-time payments will be synced to Cloud Firestore into a payments collection for the relevant customer doc if you update your webhook handler in the Stripe dashboard to include the following events: payment_intent.succeeded, payment_intent.payment_failed, payment_intent.canceled, payment_intent.processing.

To create a Checkout Session ID for a one-time payment, pass mode: 'payment to the Checkout Session doc creation:

```jsx
import { collection, addDoc, getFirestore } from "firebase/firestore";

const db = getFirestore();

async function createCheckoutSession() {
  const docRef = await addDoc(collection(db, "customers", currentUser.uid, "checkout_sessions"), {
    mode: "payment",
    price: "price_xxxxxxxxxx", // One-time price created in Stripe
    success_url: window.location.origin,
    cancel_url: window.location.origin,
  });

  console.log("Document written with ID: ", docRef.id);
}

```
> 1. **Imports:** Import collection, addDoc, and getFirestore from firebase/firestore.
> 2. **Accessing Firestore:** Use getFirestore() to retrieve the Firestore instance.
> 3. **Creating Documents:** Use addDoc to create a new document in the specified collection
> 4. **Path Construction:** Use collection(db, "customers", currentUser.uid, "checkout_sessions") to construct the path to the nested collection.
> 5. **Data Fields:** Pass the data fields as an object to addDoc.
> 6. **Return Value:** addDoc returns a DocumentReference, which you can use to access the newly created document.


## Start a subscription with Stripe Checkout
To subscribe the user to a specific pricing plan, create a new doc in the checkout_sessions collection for the user. The extension will update the doc with a Stripe Checkout session ID which you then use to redirect the user to the checkout page.

To create a Checkout Session ID for a one-time payment, pass mode: 'payment to the Checkout Session doc creation:

```jsx
import { collection, addDoc, onSnapshot, getFirestore } from "firebase/firestore";

const db = getFirestore();

async function createCheckoutSessionAndListen() {
  const docRef = await addDoc(collection(db, "customers", currentUser.uid, "checkout_sessions"), {
    price: "price_1GqIC8HYgolSBA35zoTTN2Zl",
    success_url: window.location.origin,
    cancel_url: window.location.origin,
  });

  onSnapshot(docRef, (snap) => {
    const { error, url } = snap.data();

    if (error) {
      alert(`An error occurred: ${error.message}`);
      // Handle error and inspect Cloud Function logs
    }

    if (url) {
      window.location.assign(url);
    }
  });
}

```
> 1. **Imports:** Import onSnapshot along with other Firestore functions.
> 2. **Attaching Listener:** Use onSnapshot(docRef, (snap) => { ... }) to listen for changes to the document.
> 3. **Accessing Data:** Access data fields within the snapshot as snap.data().error and snap.data().url.
> 4. **Handling Changes:** Perform actions based on the presence of error or url within the snapshot data.


