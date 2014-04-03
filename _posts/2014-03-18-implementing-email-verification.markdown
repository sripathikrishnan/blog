---
layout: post
title:  "Stateless Email Verification"
date:   2014-04-18 10:20:50
categories: security stateless
---

My preference has always been stateless approach, but the approach mentioned needs a bit more work. You want the link to expire in some time - so when you generate the signature, you append a timestamp into it as well. 

Stateless email verification should be something like this - 

1. Make a JSON Object with {"email": "sripathi@kickdrumtech.com", "timestamp": "1395139162"}
2. Base64 encode JSON Object with URL Safe Encoder. Call the string as "payload"
3. Compute a signature, which is SHA1(payload + secret key). MD5 is also fine for this purpose.
4. Generate a link - https://example.com/verify_email?payload=<payload>&signature=<signature> and email the user.
5. When the user clicks the link, extract the payload and verify the signature. If signature mismatch - raise hell
6. Extract timestamp and verify it hasn't expired
7. Finally, confirm the email. Optionally, log in the database when the email was confirmed.
