# Jenkins CI/CD Pipeline
This Jenkins pipeline automates the complete CI/CD workflow for a Java Spring Boot application, implementing continuous integration, code quality checks, containerization, and GitOps-based deployment updates.



## Pipeline Architecture

Jenkins pipeline with 5 automated stages: checkout, build & test, code analysis, Docker image creation, and manifest updates. Ensures quality gates are passed before deploying Spring Boot applications to Kubernetes via ArgoCD.


---

## Stage 1: Checkout

**Purpose:** Retrieve source code from GitHub repository

- Connects to the GitHub repository
- Pulls the latest code from the `main` branch
- Currently using a placeholder (`echo passed`) for testing
- Once ready, uncomment the git command to enable automatic code checkout

---

## Stage 2: Build and Test

**Purpose:** Compile the Java application and create deployable artifacts

- Verifies Java and Maven installations
- Navigates to the Spring Boot application directory
- Runs `mvn package` which:
  - Compiles the source code
  - Runs unit tests
  - Creates a JAR/WAR file ready for deployment

---

## Stage 3: Static Code Analysis

**Purpose:** Analyze code quality and identify bugs, vulnerabilities, and code smells

- Connects to SonarQube server at `http://13.127.89.250:9000`
- Authenticates using stored credentials (`sonar-token`)
- Scans the codebase for:
  - **Bugs** - Potential errors in the code
  - **Vulnerabilities** - Security issues
  - **Code Smells** - Maintainability issues
  - **Code Coverage** - Percentage of code tested
  - **Technical Debt** - Estimated time to fix issues

---

## Stage 4: Build and Push Docker Image

**Purpose:** Containerize the application and push it to DockerHub

1. **Docker Login:** Authenticates with DockerHub using stored credentials
2. **Build Image:** Creates a Docker container image with the application
   - Tags it with the Jenkins build number (e.g., `sakshihingankar/ultimate-cicd:45`)
   - Build number ensures each deployment has a unique, traceable version
3. **Push Image:** Uploads the image to DockerHub registry
   - Makes it available for deployment to Kubernetes

---

## Stage 5: Update Deployment File

**Purpose:** Update Kubernetes manifest with new Docker image version (GitOps approach)

**What it does:**

1. **Configure Git Identity:** Sets up git user for commits
2. **Switch to Main Branch:** Ensures we're on the correct branch (not detached HEAD)
3. **Update Manifest:** Uses `sed` to replace the Docker image tag in `deployment.yml`
   - Changes from old version to new `${BUILD_NUMBER}` version
4. **Commit Changes:** Only commits if there are actual changes
5. **Push to GitHub:** Updates the manifest repository

---

## Complete Workflow Visualization

```
Developer Push → GitHub Webhook → Jenkins Pipeline Trigger
                                          ↓
                                    [Checkout Code]
                                          ↓
                                    [Build with Maven]
                                          ↓
                                    [SonarQube Scan]
                                          ↓
                                    [Build Docker Image]
                                          ↓
                                    [Push to DockerHub]
                                          ↓
                                    [Update deployment.yml]
                                          ↓
                                    [Push to GitHub]
                                          ↓
                                    ArgoCD Detects Change
                                          ↓
                                    [Deploy to Kubernetes]
                                          ↓
                                    Application Updated! 
```

