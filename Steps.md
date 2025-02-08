To set up a private repository between **GitHub** and **Jenkins**, follow these steps:

---

### **1. Create a Personal Access Token (PAT) in GitHub**
Since Jenkins cannot authenticate using a password, you need a **Personal Access Token (PAT)**:

1. Go to **GitHub** > Click on your profile > **Settings**.
2. Navigate to **Developer settings** > **Personal Access Tokens** > **Fine-grained tokens**.
3. Click **Generate new token**.
4. Give it a **descriptive name** (e.g., "Jenkins-GitHub Access").
5. Set expiration or **No Expiration** (not recommended for security reasons).
6. Under **Repository Access**, select **Only select repositories** (choose your private repo).
7. Grant **permissions**:
   - `Read & write` on **Contents** (if Jenkins needs to push).
   - `Read-only` on **Metadata** (for repository access).
8. Click **Generate Token**, then **Copy** it (you won’t see it again).

---

### **2. Configure Credentials in Jenkins**
Now, you need to store the token securely in Jenkins.

1. Open **Jenkins** > Go to **Manage Jenkins** > **Manage Credentials**.
2. Under **Global credentials (unrestricted)**, click **Add Credentials**.
3. Select:
   - **Kind**: `Username with password`
   - **Username**: `Your GitHub username`
   - **Password**: `Your GitHub PAT (from Step 1)`
   - **ID**: `github-private-repo`
   - **Description**: `GitHub Private Repo Token for Jenkins`
4. Click **OK**.

---

### **3. Connect Jenkins with GitHub**
#### **Option 1: Configure GitHub in Jenkins (Globally)**
1. Go to **Manage Jenkins** > **Configure System**.
2. Scroll to **GitHub** section.
3. Click **Add GitHub Server** > **GitHub Server**.
4. Under **Credentials**, select the **GitHub PAT credentials** you added earlier.
5. Click **Test Connection** (should return a green checkmark ✅).
6. Click **Save**.

#### **Option 2: Configure Git in a Job (Per-Project)**
1. Create or open a **Jenkins Job** (Freestyle or Pipeline).
2. Under **Source Code Management**, select **Git**.
3. Enter **Repository URL** (format: `https://github.com/your-username/your-private-repo.git`).
4. Under **Credentials**, select **github-private-repo**.
5. Branches to build> Branch Specifier> (Leave empty)
6. Build Steps>Execute shell> command: bash test.sh
7. Click **Save**.

---

### **4. Configure Webhook for Automatic Build Trigger**
1. In **GitHub**, go to your **Private Repository** > **Settings** > **Webhooks**.
2. Click **Add Webhook**.
3. **Payload URL**: `http://your-jenkins-url/github-webhook/`
4. **Content Type**: `application/json`
5. **Trigger events**: Select `Just the push event` or `Pull Request` (based on your needs).
6. Click **Add Webhook**.

---

### **5. Test the Setup**
- Go to your Jenkins job and click **Build Now**.
- If the webhook is set up correctly, pushing to GitHub should automatically trigger a build in Jenkins.

---

### **Done! 🚀**
Now, Jenkins can securely pull from your private GitHub repository using the **Personal Access Token (PAT)**. Let me know if you need additional configurations like **multi-branch pipelines**, **deployments**, or **post-build actions**!
