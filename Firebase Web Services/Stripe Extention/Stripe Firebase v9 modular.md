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
##List available products and prices
> Products and pricing information are normal collections and docs in your Cloud Firestore and can be queried as such:

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
> 1. Imports: Import specific functions from firebase/firestore.
> 2. Accessing Firestore: Use getFirestore() to retrieve the Firestore instance.
> 3. Querying Collections: Use collection(db, "products") to access the "products" collection.
> 4. Filtering Data: Pass a where clause to getDocs to filter documents.
> 5. Retrieving Documents: Use getDocs to fetch documents matching the query.
> 6. Retrieving Subcollections: Use collection(doc.ref, "prices") to access the "prices" subcollection.
> 7. Iterating Over Documents: Use forEach to loop through documents and subcollection documents.
