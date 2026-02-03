Here's the HTML code - you can copy it directly:
html<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jenkins Pipeline Overview</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
            line-height: 1.6;
            color: #333;
            background-color: #f5f5f5;
            padding: 20px;
        }

        .container {
            max-width: 900px;
            margin: 0 auto;
            background: white;
            padding: 40px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }

        h1 {
            color: #2c3e50;
            font-size: 2.5em;
            margin-bottom: 20px;
            border-bottom: 3px solid #3498db;
            padding-bottom: 10px;
        }

        .intro {
            font-size: 1.1em;
            color: #555;
            margin-bottom: 40px;
            line-height: 1.8;
        }

        h2 {
            color: #2980b9;
            font-size: 1.8em;
            margin-top: 40px;
            margin-bottom: 15px;
        }

        .purpose {
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 10px;
            font-size: 1.05em;
        }

        ul {
            margin-left: 20px;
            margin-bottom: 20px;
        }

        ul li {
            margin-bottom: 8px;
            line-height: 1.6;
        }

        ul ul {
            margin-top: 8px;
        }

        ol {
            margin-left: 20px;
            margin-bottom: 20px;
        }

        ol li {
            margin-bottom: 12px;
            line-height: 1.6;
        }

        code {
            background-color: #f4f4f4;
            padding: 2px 6px;
            border-radius: 3px;
            font-family: 'Courier New', monospace;
            color: #e74c3c;
            font-size: 0.9em;
        }

        strong {
            color: #2c3e50;
        }

        .workflow {
            background-color: #f8f9fa;
            padding: 30px;
            border-radius: 8px;
            margin-top: 30px;
            border-left: 4px solid #3498db;
        }

        .workflow h2 {
            margin-top: 0;
        }

        pre {
            background-color: #2c3e50;
            color: #ecf0f1;
            padding: 20px;
            border-radius: 6px;
            overflow-x: auto;
            line-height: 1.8;
            font-family: 'Courier New', monospace;
            font-size: 0.95em;
        }

        @media (max-width: 768px) {
            .container {
                padding: 20px;
            }

            h1 {
                font-size: 2em;
            }

            h2 {
                font-size: 1.5em;
            }

            pre {
                font-size: 0.85em;
                padding: 15px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Jenkins Pipeline Overview</h1>
        
        <p class="intro">
            This Jenkins pipeline automates the complete CI/CD workflow for a Java Spring Boot application, implementing continuous integration, code quality checks, containerization, and GitOps-based deployment updates.
        </p>

        <h2>Stage 1: Checkout</h2>
        <p class="purpose">Purpose: Retrieve source code from GitHub repository</p>
        <ul>
            <li>Connects to the GitHub repository</li>
            <li>Pulls the latest code from the <code>main</code> branch</li>
            <li>Currently using a placeholder (<code>echo passed</code>) for testing</li>
            <li>Once ready, uncomment the git command to enable automatic code checkout</li>
        </ul>

        <h2>Stage 2: Build and Test</h2>
        <p class="purpose">Purpose: Compile the Java application and create deployable artifacts</p>
        <ul>
            <li>Verifies Java and Maven installations</li>
            <li>Navigates to the Spring Boot application directory</li>
            <li>Runs <code>mvn package</code> which:
                <ul>
                    <li>Compiles the source code</li>
                    <li>Runs unit tests</li>
                    <li>Creates a JAR/WAR file ready for deployment</li>
                </ul>
            </li>
        </ul>

        <h2>Stage 3: Static Code Analysis</h2>
        <p class="purpose">Purpose: Analyze code quality and identify bugs, vulnerabilities, and code smells</p>
        <ul>
            <li>Connects to SonarQube server at <code>http://13.127.89.250:9000</code></li>
            <li>Authenticates using stored credentials (<code>sonar-token</code>)</li>
            <li>Scans the codebase for:
                <ul>
                    <li><strong>Bugs</strong> - Potential errors in the code</li>
                    <li><strong>Vulnerabilities</strong> - Security issues</li>
                    <li><strong>Code Smells</strong> - Maintainability issues</li>
                    <li><strong>Code Coverage</strong> - Percentage of code tested</li>
                    <li><strong>Technical Debt</strong> - Estimated time to fix issues</li>
                </ul>
            </li>
        </ul>

        <h2>Stage 4: Build and Push Docker Image</h2>
        <p class="purpose">Purpose: Containerize the application and push it to DockerHub</p>
        <ol>
            <li><strong>Docker Login:</strong> Authenticates with DockerHub using stored credentials</li>
            <li><strong>Build Image:</strong> Creates a Docker container image with the application
                <ul>
                    <li>Tags it with the Jenkins build number (e.g., <code>sakshihingankar/ultimate-cicd:45</code>)</li>
                    <li>Build number ensures each deployment has a unique, traceable version</li>
                </ul>
            </li>
            <li><strong>Push Image:</strong> Uploads the image to DockerHub registry
                <ul>
                    <li>Makes it available for deployment to Kubernetes</li>
                </ul>
            </li>
        </ol>

        <h2>Stage 5: Update Deployment File</h2>
        <p class="purpose">Purpose: Update Kubernetes manifest with new Docker image version (GitOps approach)</p>
        <p><strong>What it does:</strong></p>
        <ol>
            <li><strong>Configure Git Identity:</strong> Sets up git user for commits</li>
            <li><strong>Switch to Main Branch:</strong> Ensures we're on the correct branch (not detached HEAD)</li>
            <li><strong>Update Manifest:</strong> Uses <code>sed</code> to replace the Docker image tag in <code>deployment.yml</code>
                <ul>
                    <li>Changes from old version to new <code>${BUILD_NUMBER}</code> version</li>
                </ul>
            </li>
            <li><strong>Commit Changes:</strong> Only commits if there are actual changes</li>
            <li><strong>Push to GitHub:</strong> Updates the manifest repository</li>
        </ol>

        <div class="workflow">
            <h2>Complete Workflow Visualization</h2>
            <pre>
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
            </pre>
        </div>
    </div>
</body>
</html>
