## Chapter 6: Sensitive Data Protection, Secret Manager, and Data Service Access Controls — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Secret Manager maximum secret payload size? | 64 KiB (65,536 bytes) per secret version. Exam tip: larger secrets must be stored elsewhere (e.g., Cloud Storage) with a reference stored in Secret Manager. |
| 2 | How long can you revert uniform bucket-level access after enabling it on a Cloud Storage bucket? | 90 days. After that window, the setting becomes permanent and ACLs cannot be re-enabled. |
| 3 | How many likelihood levels does Sensitive Data Protection (DLP) use when classifying infoType matches? | 5 levels: VERY_UNLIKELY, UNLIKELY, POSSIBLE, LIKELY, VERY_LIKELY. Exam tip: inspection rules can filter findings by minimum likelihood. |
| 4 | Gotcha with Cloud SQL IAM database authentication on MySQL? | MySQL usernames are capped at 32 characters — long IAM principal emails get truncated, potentially causing collisions or failed logins. PostgreSQL does not have this limitation. |
| 5 | Requirement for a BigQuery authorized view to work? | The view must reside in a dataset different from the source table's dataset, and that view (not the end user) is granted access to query the source table. |
| 6 | CryptoHashConfig vs CryptoReplaceFfxFpeConfig — which is reversible? | CryptoHashConfig = irreversible one-way hash (fixed output, no format preservation). CryptoReplaceFfxFpeConfig = reversible format-preserving encryption using a wrapped KMS key. |
| 7 | Cloud Storage IAM vs ACLs — key architectural difference? | IAM applies at project/bucket level to broad roles; ACLs apply per-object/bucket for fine-grained individual grants. Enabling uniform bucket-level access disables ACLs entirely, leaving only IAM. |
| 8 | Name the three key Secret Manager IAM roles and their purpose. | roles/secretmanager.admin (full control), roles/secretmanager.secretAccessor (read secret payload only), roles/secretmanager.secretVersionManager (enable/disable/destroy versions, no payload access). |
| 9 | Basic syntax to create BigQuery row-level security? | CREATE ROW ACCESS POLICY policy_name ON dataset.table GRANT TO ("user:name@example.com") FILTER USING (region = 'US'); — filters rows returned to the grantee. |
| 10 | How is BigQuery column-level security implemented? | Via policy tags defined in a Data Catalog/Dataplex taxonomy applied to schema columns; users need the 'Fine-Grained Reader' role on the specific policy tag to see that column's data. |
| 11 | Secret Manager: automatic vs user-managed replication? | Automatic = Google selects storage locations automatically for availability (default, simplest). User-managed = you explicitly specify the list of regions, required for data residency compliance. |
| 12 | What does CharacterMaskConfig control in DLP de-identification? | Replaces characters with a masking character (e.g., 'X' or '*'); parameters let you specify how many characters to leave unmasked from the start or end, e.g., showing only the last 4 digits of a card number. |
