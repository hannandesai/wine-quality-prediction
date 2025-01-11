# Wine Quality Prediction App

## Overview

This project demonstrates the use of Apache Spark on an AWS EMR Cluster to train a machine learning model in parallel across multiple EC2 instances (4 in this case). The trained model predicts the quality of wine based on input data. The workflow includes saving the model to an Amazon S3 bucket and using it in a prediction application.

## Applications

### 1. Model Training Application
- **Description**: This application trains a machine learning model using the Linear Regression algorithm and saves the trained model to an Amazon S3 bucket.
- **Repository**: [GitHub - wineprediction-model-training](https://github.com/hannandesai/wineprediction-model-training.git)
- **Run Instructions**:
  ```bash
  mvn clean package
  mvn exec:java -Dexec.mainClass="org.example.wineprediction.App"
  ```
- **Execution Environment**: Amazon EMR Cluster with Apache Spark on 4 EC2 instances.

### 2. Prediction Application
- **Description**: This application uses the trained model from the S3 bucket to predict wine quality using input data from `TestData.csv`.
- **Repository**: [GitHub - winepredictionapp](https://github.com/hannandesai/winepredictionapp.git)
- **Docker Hub**: [Docker Image](https://hub.docker.com/repository/docker/hannan9900/winepredictionapp/general)
- **Run Instructions**:
  ```bash
  mvn clean package
  mvn exec:java -Dexec.mainClass="org.example.winepredictionapp.App"
  ```
- **Execution Environment**: Single EC2 instance using a Docker image.

---

## Cloud Setup

### AWS EMR Cluster
1. **Create the EMR Cluster**
   - Follow AWS documentation to set up an EMR cluster.
   - Ensure the cluster has 4 EC2 instances for parallel processing.

### Amazon S3 Bucket
1. **Create an S3 Bucket**
   - Store the input CSV files (`TrainingDataSet.csv` and `ValidationDataSet.csv`) and the trained ML model.

### Running the Model Training Application
1. **Connect to the EMR Cluster**
   - Use SSH to access the cluster.

2. **Package the Application**
   ```bash
   mvn clean package
   ```
   - This creates the JAR file: `wineprediction-1.0-SNAPSHOT-jar-with-dependencies.jar` in the `target` folder.

3. **Upload JAR File to S3**
   - Upload the JAR file to the S3 bucket created earlier.

4. **Submit the Job to Apache Spark**
   ```bash
   spark-submit s3://<bucket-name>/wineprediction-1.0-SNAPSHOT-jar-with-dependencies.jar
   ```

5. **Monitor Job Execution**
   - Use the Spark History Server UI to view the job status.

6. **Output**
   - The trained model (`LinearRegressionModel`) is saved to the S3 bucket.

### Docker Image Creation
1. **Create a Dockerfile**
   - Place the Dockerfile in the root directory of the `winepredictionapp` project.

2. **Build the Docker Image**
   ```bash
   docker build -t <image-name> .
   ```

3. **Push the Image to Docker Hub**
   ```bash
   docker push <dockerhub-username>/<image-name>
   ```

### Running the Prediction Application
1. **Using Docker**
   1. Connect to the EC2 instance.
   2. Install Docker on the instance.
   3. Pull the Docker image from Docker Hub:
      ```bash
      docker pull <dockerhub-username>/<image-name>
      ```
   4. Run the Docker image:
      ```bash
      docker run -it <dockerhub-username>/<image-name>
      ```

2. **Without Docker**
   - Run the following commands in the project root directory:
     ```bash
     mvn clean package
     mvn exec:java -Dexec.mainClass="org.example.winepredictionapp.App"
     ```

---

## Contributor
**Mohammed Hannan Desai**

---

## License
This project is licensed under the MIT License.
