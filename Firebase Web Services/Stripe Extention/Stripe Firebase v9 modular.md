
```

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
