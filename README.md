Implements a secure account deletion feature with multiple security safeguards. Users can delete their accounts through a modal interface that requires additional verification steps. The implementation uses a soft-deletion approach with a 30-day recovery window before permanent deletion. Security measures include password verification, email confirmation, and audit logging of all deletion events.

Key implementations:
- Enhanced `AccountDeletionModal` with multi-step verification
- Added password re-verification before processing deletion
- Implemented email confirmation flow for deletion requests
- Created a deletion queue service with 30-day retention period
- Added audit logging for all deletion-related actions
- Updated JWT strategy to handle restoration of accounts within the recovery window

## 🏁 Test plan

### 🧪 Test Scenario 1: `Secure Account Deletion Flow`

**Objective**: Verify that users must complete all security steps to delete their account.

**Steps**:
1. Log in to the application
2. Navigate to account settings
3. Click the "Delete Account" button
4. Verify the modal displays proper security warnings
5. Enter account password for verification
6. Confirm deletion intent by typing "DELETE" in the confirmation field
7. Click "Request Account Deletion" button
8. Verify a confirmation email is sent
9. Click the verification link in the email
10. Verify user is logged out and informed about the 30-day recovery window

**Expected Outcome**: Account should be marked as pending deletion, and the deletion should only be completed after proper verification and the waiting period.

---

### 🧪 Test Scenario 2: `Cancellation During Recovery Window`

**Objective**: Verify that users can cancel account deletion during the 30-day recovery window.

**Steps**:
1. Complete the account deletion request process
2. Within the 30-day window, log in with account credentials
3. Verify user is presented with a "Restore Account" option
4. Click "Restore Account"

**Expected Outcome**: The account deletion should be canceled, and the account should be fully restored to active status.

---

### 🧪 Test Scenario 3: `Failed Password Verification`

**Objective**: Verify that incorrect password prevents account deletion.

**Steps**:
1. Initiate the account deletion process
2. Enter an incorrect password in the verification field
3. Click "Request Account Deletion"

**Expected Outcome**: The system should reject the request and display an error message about incorrect password.

---

### 🧪 Test Scenario 4: `Rate Limiting Protection`

**Objective**: Verify that rate limiting prevents abuse of the deletion feature.

**Steps**:
1. Attempt to initiate account deletion multiple times in quick succession

**Expected Outcome**: After a certain number of attempts, the system should temporarily block further deletion requests and display an appropriate message.

---

### 🧪 Test Scenario 5: `Audit Trail Verification`

**Objective**: Verify that all deletion-related actions are properly logged.

**Steps**:
1. Complete the account deletion process
2. As an admin, check the audit logs

**Expected Outcome**: The logs should contain detailed entries for each step of the deletion process, including initiation, email verification, and final deletion status.

---

### 🧪 Test Scenario 6: `Automatic Permanent Deletion After 30 Days`

**Objective**: Verify that accounts are permanently deleted after the recovery window.

**Steps**:
1. Complete the account deletion process
2. Simulate the passage of 30 days
3. Verify the account data is permanently removed as per privacy policy

**Expected Outcome**: After 30 days, the account should be permanently deleted from the system (or anonymized per data protection requirements).
