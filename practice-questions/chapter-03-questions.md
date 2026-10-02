## Chapter 3: IAM Authorization: Conditions, Deny Policies, and Org Policy — Practice Questions

**Q1.** A company wants contractors to access Compute Engine resources only between 9am and 5pm on weekdays, without writing any external scheduling tools. Which feature should the security engineer configure?

A) An IAM condition on the role binding using a request.time CEL expression
B) An IAM deny policy scoped to the contractors' group
C) An Access Context Manager access level based on time of day
D) An organization policy list constraint restricting login hours

<details>
<summary>Answer</summary>

**Correct: A**

IAM conditions use CEL expressions against request.time to grant attribute-based, time-bound access directly on a role binding.

**Why the others are wrong:**
- Deny policies block access regardless of role and are not designed as the primary time-window granting mechanism; they don't attach time-based allow logic.
- Access Context Manager access levels evaluate IP, device, and geography — not time of day.
- Org policy constraints govern resource configuration, not per-principal access windows.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A security team must guarantee that no principal in the organization — including Organization Administrators — can create service account keys, except for a designated break-glass emergency group. Which control best satisfies this requirement?

A) A predefined role removal at the organization level
B) An organization policy boolean constraint enforcing key creation disabled
C) A custom role excluding the iam.serviceAccountKeys.create permission
D) An IAM deny policy at the organization node with the break-glass group listed as exceptionPrincipals

<details>
<summary>Answer</summary>

**Correct: D**

Deny policies explicitly block a permission org-wide for everyone, overriding any allow — including for admins — while exceptionPrincipals lets you carve out the break-glass identity.

**Why the others are wrong:**
- Removing predefined roles doesn't stop admins who hold broader roles like Owner from still having the permission via other bindings.
- A boolean org policy constraint blocks the API behavior globally but has no clean per-group exception mechanism for a break-glass identity.
- Custom roles only affect principals granted that specific role; it doesn't stop other roles/bindings from granting the same permission.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** An organization policy boolean constraint disabling service account key creation is set at the organization node. A project owner who holds the Organization Policy Administrator role at the project level sets an overriding policy on their project to allow key creation for testing. What happens?

A) The project-level policy takes effect for that project because a policy closer to the resource in the hierarchy overrides the inherited one
B) The organization-level policy always wins regardless of any project-level setting
C) Both policies apply simultaneously and the most restrictive setting always wins
D) The override is rejected automatically because boolean constraints can never be overridden below the organization node

<details>
<summary>Answer</summary>

**Correct: A**

Organization policies follow a hierarchy-inheritance model where a policy set closer to the resource replaces the inherited policy, unlike IAM deny policies which can never be overridden by a lower-level allow.

**Why the others are wrong:**
- This ignores how org policy inheritance actually works and confuses it with deny-policy behavior.
- Org policies do not merge to the most restrictive setting by default — that requires the newer conditional/hierarchical rule authoring, not the default model.
- Boolean constraints can absolutely be overridden at a lower node by a principal with orgpolicy.policyAdmin permission, which is exactly the trap the exam tests.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Employees must be able to reach the Google Cloud console only from corporate-managed devices located in the United States, regardless of what IAM roles they hold. Which service enforces this?

A) Access Context Manager access levels used with context-aware access
B) IAM Conditions attached to each role binding
C) VPC Service Controls perimeters
D) Organization Policy Service constraints

<details>
<summary>Answer</summary>

**Correct: A**

Access Context Manager access levels evaluate device policy and geographic/IP attributes and are enforced through context-aware access (Identity-Aware Proxy) independent of the IAM role granted.

**Why the others are wrong:**
- IAM conditions can reference some request attributes but are not the designed mechanism for device-trust and geography enforcement across the console.
- VPC Service Controls create data perimeters around API access to services, not console login device/location checks.
- Org Policy constraints govern resource configuration behavior, not user device or location-based console access.

</details>

---

**Q5.** A responder needs temporary elevated access to a production project for 4 hours during an incident, with an approval workflow and full audit trail, without permanently modifying existing IAM bindings. Which service should be used?

A) An IAM condition with a fixed expiration timestamp
B) Privileged Access Manager (PAM) entitlement grant
C) A custom role scoped to the incident
D) A deny policy exception for the responder

<details>
<summary>Answer</summary>

**Correct: B**

PAM is purpose-built for just-in-time, time-bound privileged access grants with approval workflows and automatic revocation, leaving no permanent IAM binding behind.

**Why the others are wrong:**
- An IAM condition expiration still requires manually creating and later removing a binding and has no built-in approval workflow.
- A custom role changes what permissions exist, not who has time-bound access to them, and requires manual grant/revoke.
- A deny policy exception removes a restriction; it does not grant new elevated access.

</details>

---

**Q6.** You need to restrict where new Compute Engine VMs can be created to only us-central1 and us-east1 across the organization. Which organization policy constraint type do you configure?

A) A boolean constraint set to enforced
B) An IAM condition on the compute.instances.create permission
C) A deny policy scoped to all other regions
D) A list constraint with allowed_values set to the two regions

<details>
<summary>Answer</summary>

**Correct: D**

constraints/gcp.resourceLocations is a list constraint that accepts allowed_values/denied_values to restrict resource creation locations.

**Why the others are wrong:**
- Boolean constraints only support enforced/not-enforced, no value list, so they cannot express a set of allowed regions.
- IAM conditions govern who can act on a resource, not where a resource can be provisioned.
- Deny policies restrict permissions for principals, not the configuration values (like location) a resource can take.

</details>

---

**Q7.** Before removing a broad IAM role binding, a security engineer wants to see exactly which principals would lose access and what would break, without actually applying the change. Which Policy Intelligence tool should be used?

A) Policy Analyzer
B) IAM Recommender
C) Policy Simulator
D) Access Context Manager

<details>
<summary>Answer</summary>

**Correct: C**

Policy Simulator lets you test a proposed IAM policy change and shows the resulting access differences before deployment.

**Why the others are wrong:**
- Policy Analyzer answers 'who currently has access to what' — it inspects existing state, it does not simulate a proposed change.
- IAM Recommender proactively suggests removing unused permissions based on usage; it doesn't let you test an arbitrary hypothetical change.
- Access Context Manager configures context-aware access levels and has nothing to do with IAM policy diffing.

</details>

---

**Q8.** What is the key functional difference between an IAM allow policy and an IAM deny policy?

A) Deny policies apply only to service accounts, never to users or groups
B) Deny policies can only be attached at the project level, never at org or folder
C) Allow policies grant permissions and are inherited additively down the resource hierarchy, while deny policies explicitly block permissions and always take precedence over any allow
D) Allow policies always override deny policies when the principal holds the Owner role

<details>
<summary>Answer</summary>

**Correct: C**

This is the core IAM authorization model: allow policies union permissions down the hierarchy; deny policies are evaluated first and any explicit deny wins, regardless of role or hierarchy level.

**Why the others are wrong:**
- Deny policies can target any principal type, not just service accounts.
- Deny policies can be attached at organization, folder, or project nodes, just like allow policies.
- Explicit deny always wins, even for Owner — this is precisely the safeguard deny policies provide.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

