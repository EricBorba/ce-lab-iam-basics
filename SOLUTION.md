# IAM Basics Lab - Solution

**Student Name:** [Eric Rodrigues Borba]  
**Date Completed:** [21/04/2026]

---

## Exercise 1: IAM Groups

### Screenshots:
![Groups Created](screenshots/groups-created.png)

### Groups Created:
- [x] Developers group
- [x] DevOps group

---

## Exercise 2: Group Permissions

### Developers Group:
![Developers Permissions](screenshots/developers-permissions.png)

**Policies Attached:**
- AmazonS3FullAccess
- AmazonEC2ReadOnlyAccess

### DevOps Group:
![DevOps Permissions](screenshots/devops-permissions.png)

**Policies Attached:**
- AmazonS3FullAccess
- AmazonEC2FullAccess

---

## Exercise 3: IAM Users

### Screenshots:
![Users Created](screenshots/users-created.png)

### Users Created:

| Username | Group | Console Access | Status |
|----------|-------|----------------|--------|
| alice | Developers | Yes | ✅ Created |
| bob | Developers | Yes | ✅ Created |
| charlie | DevOps | Yes | ✅ Created |

---

## Exercise 4: Permission Testing

### Alice's Access Tests:

**S3 Access:**
![Alice S3 Access](screenshots/alice-s3-access.png)
- Create bucket: ✅ SUCCESS
- Upload file: ✅ SUCCESS

**EC2 Access:**
![Alice EC2 Read-Only](screenshots/alice-ec2-readonly.png)
- View instances: ✅ SUCCESS
- Launch instance: ❌ DENIED (Expected)

### Bob's Access Tests:

**S3 Access:**
![Bob S3 Access](screenshots/bob-s3-access.png)
- Create bucket: ✅ SUCCESS

**EC2 Access:**
![Bob EC2 Denied](screenshots/bob-ec2-denied.png)
- View instances: ✅ SUCCESS
- Launch instance: ❌ DENIED (Expected)

### Charlie's Access Tests:

**Full Access:**
![Charlie Full Access](screenshots/charlie-full-access.png)
- S3 create bucket: ✅ SUCCESS
- EC2 launch instance: ✅ SUCCESS

### Summary of Test Results:

| User | S3 Full | EC2 View | EC2 Launch | Result |
|------|---------|----------|------------|--------|
| alice | ✅ | ✅ | ❌ | As expected |
| bob | ✅ | ✅ | ❌ | As expected |
| charlie | ✅ | ✅ | ✅ | As expected |

---

## Exercise 5: Custom Policy

### Policy JSON:
![Custom Policy](screenshots/custom-policy.png)

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ListAllBuckets",
            "Effect": "Allow",
            "Action": "s3:ListAllMyBuckets",
            "Resource": "*"
        },
        {
            "Sid": "DevBucketAccess",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::dev-bucket-ericborba",
                "arn:aws:s3:::dev-bucket-ericborba/*"
            ]
        }
    ]
}
```

### Custom Policy Test:
![Bob Custom Policy Test](screenshots/bob-custom-policy-test.png)

**Bob's Access After Custom Policy:**
- Access dev-bucket: ✅ SUCCESS
- Access other buckets: ❌ DENIED (Expected)

---

## Exercise 6: MFA Configuration

![MFA Enabled](screenshots/mfa-enabled.png)

**MFA Details:**
- User: [alice]
- Device type: Virtual MFA
- Authenticator app: [Microsoft Authenticator]
- Status: ✅ Active

---

## Bonus Challenges

### Challenge 1: Password Policy

![Password Policy](screenshots/password-policy.png)

**Policy Settings:**
- [x] Minimum length: 12 characters
- [x] Require uppercase letters
- [x] Require lowercase letters
- [x] Require numbers
- [x] Require symbols
- [x] Password expiration: 90 days

---

### Challenge 2: Access Analyzer

![Access Analyzer](screenshots/access-analyzer.png)

**Findings:**
- Number of findings: [X]
- Critical issues: [List any public access found]
- Recommendations: [Your notes]

---

### Challenge 3: CLI Access Keys

**Alice Access Key Created:** [Yes]

**CLI Test Output:**
```bash
$ aws s3 ls --profile alice
[(base) ericborba@Erics-Air ce-lab-iam-basics % aws s3 ls --profile alice
2026-04-21 14:45:04 alice-ironhack-myfirstbucket
2026-04-21 14:50:48 bob-ironhack-myfirstbucket
2026-04-21 14:54:08 charlie-ironhack-myforstbucket
2026-04-21 15:05:35 dev-bucket-ericborba
]
```

**Screenshot:** [If applicable]
![Alice CLI](screenshots/alice-cli.png)
---

## Reflection Questions

### 1. Why use groups instead of attaching policies directly to users?

**Your Answer:**

[Groups let you assign permissions to multiple users at once (e.g., developers) instead of doing it one by one. If something changes, you update the policy in the group and everyone inside automatically gets the update. It's faster and cleaner.]

---

### 2. What are the risks of giving everyone AdministratorAccess?

**Your Answer:**

[Everyone would have full power over your AWS account being capable of deleting resources, modifying services, assigning privileges, even wiping out data. One mistake or one compromised account could take down your entire infrastructure. That's why you follow the principle of least privilege.]

---

### 3. How would you organize IAM for 50 developers across 5 projects?

**Your Answer:**

[I'd create one group per project and assign each developer to the group that matches their project. That way permissions are scoped to what each team actually needs, and managing access changes stays simple and clean.]

---

### 4. What happens if you delete an IAM user? Can you recover their permissions?

**Your Answer:**

[Deleting an IAM user is permanent (the user and their credentials are gone). You can't recover them, but you can recreate the user and reattach the same policies. What you lose forever though is the activity history and any access keys that were tied to that user.]

---

## Key Learnings

**What was most challenging about this lab?**

[Staying focused while assigning policies because there are so many of them that it's easy to accidentally attach the wrong one to the wrong user.]

---

**What IAM best practice will you always follow?**

[Least privilege, without a doubt. Give users only what they need to do their job]

---

**How does IAM help implement the principle of least privilege?**

[IAM lets you control exactly what each user, group, or role can and can't do through policies. Instead of giving broad access, you attach only the specific permissions someone needs]

---

## Checklist

- [x] All 3 users created (alice, bob, charlie)
- [x] Both groups created (Developers, DevOps)
- [x] Permissions tested for each user
- [x] Custom policy created and tested
- [x] MFA enabled for at least one user
- [x] All screenshots captured
- [x] All reflection questions answered
- [x] Policy JSON file saved
- [x] Work committed to Git
- [x] Pull request created

---

**Completed By:** [Eric Rodrigues Borba]  
**Date:** [21/04/2026]
