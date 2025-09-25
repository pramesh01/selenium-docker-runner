# 🚀 Selenium-Docker-Runner

This repository contains the **runner setup** to execute the Selenium Automation Framework using **Docker** and **Jenkins**.  
It provisions a Selenium Grid, pulls the prebuilt Docker image from DockerHub, runs tests in parallel on multiple browsers, collects reports, and then tears down the environment automatically.

---
## 📂 Repository Contents
- **Jenkinsfile** → Pipeline to orchestrate test execution (Grid up → Run tests → Cleanup).  
- **grid.yaml** → Spins up Selenium Grid (Hub + Chrome + Firefox nodes).  
- **test_suites.yaml** → Runs test containers against the Grid in parallel.  
---

## ⚙️ Workflow

### 🔹 Builder Pipeline (Jenkinsfile defined in my [Selenium Framework Repository](https://github.com/pramesh01/selenium-dockerized-test-automation-framework))
The Docker image used here (`pramesh11/selenium-docker-framework`) is built and pushed from the main framework repository, which contains:
- Complete framework code (Selenium + Java + TestNG + POM).  
- `pom.xml` for Maven build.  
- **Builder Jenkinsfile** with stages to:
  1. Build the JAR (`mvn clean package`).  
  2. Build Docker image.  
  3. Push image to DockerHub (`pramesh11/selenium-docker-framework`).  

This pipeline must run **first**, so that the latest Docker image is available on DockerHub.

### 🔹 Runner Job (this Repository)
- **Grid Up** → `grid.yaml` starts Selenium Hub and browser nodes.  
- **Test Execution** → `test_suites.yaml` runs tests using the prebuilt Docker image:
  - Runs in **Chrome** and **Firefox**.  
  - Stores reports in `./output/test_resultsX`.  
- **Validation** → Jenkins pipeline checks for failed tests (`testng-failed.xml`).  
- **Teardown** → All containers are stopped and cleaned automatically.  

---
## ▶️ Usage

### Run with Jenkins
- Two pipelines are needed:
  1. **Builder Job** (from framework repo) → Builds & pushes Docker image.  
  2. **Runner Job** (this repo) → Executes tests on Selenium Grid.  

- Run the **Builder Job first**, then the **Runner Job**.  

### Run Manually (Docker Compose)
```bash
# Start Selenium Grid
docker-compose -f grid.yaml up -d

# Run tests (Chrome + Firefox)
docker-compose -f test_suites.yaml up --pull=always

# Stop and clean everything
docker-compose -f test_suites.yaml down
docker-compose -f grid.yaml down

📊 **Test Reports**

 Reports are generated inside the output/ directory:-

  - output/test_results1 → Chrome execution results
  - output/test_results2 → Firefox execution results

👨‍💻 **Author: Pramesh Kumar**

Thanks!
