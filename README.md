# CBIO312-PROJECT

**"Mini-HPC and Hybrid HPC-Big Data Clusters"**, developed as part of the **CBIO312 - High Performance Computing (HPC)** course.

## Project Overview

The goal of this project is to:
- Set up a **3-node virtual cluster** using VirtualBox.
- Implement **distributed machine learning tasks** using **MPI/mpi4py**.
- Deploy a **Docker Swarm + Apache Spark** cluster for **Big Data processing**.
- Evaluate performance on real-world **bioinformatics gene expression data**.

---

##  Team Members

| Name             | ID           |
|------------------|--------------|
| Inji Hatem Amin  | 231000596    |
| Rowan Atef       | 231001403    |
| Amira Momtaz     | 231001071    |

**Supervised by:** Dr. Mohamed Mahmoud El Sayeh  
**Teaching Assistant:** Malak Soliman

---

## Repository Structure

```
project_hpc_hybrid_cluster/
├── README.md
├── hostfile                      # MPI hostfile listing nodes and slots
├── distributed_mnist.py          # Distributed ML script using digits dataset
├── distributed_gene_analysis.py  # Distributed bioinformatics analysis using MPI
├── spark-stack.yml               # Docker Compose file for Spark cluster
├── distributed_gene_expression_analysis.py  # PySpark ML script
├── bioinfo_data/
│   └── leukemia_expression.csv   # Real-world gene expression dataset
├── screenshots/                  # Screenshots of key steps and outputs
├── Final_Report.pdf              # Full academic report of the project
└── ml_job.slurm (optional)       # SLURM job submission script (optional)
```

---

## Task Breakdown

### **Task 1: Mini-HPC Cluster (Traditional HPC with MPI)**

#### Objectives:
- Configure a 3-node cluster (1 master + 2 workers).
- Enable passwordless SSH between nodes.
- Run distributed ML tasks using OpenMPI and mpi4py.
- Use both synthetic (digits) and real (leukemia gene expression) datasets.

#### Setup Instructions:
1. Import VMs in VirtualBox.
2. Assign static IPs:  
   - Master: `192.168.56.10`  
   - Worker1: `192.168.56.11`  
   - Worker2: `192.168.56.12`
3. Generate and distribute SSH keys for passwordless login.
4. Install OpenMPI and Python dependencies.
5. Run scripts using `mpirun`:

```bash
mpirun --hostfile hostfile -n 6 python3 distributed_mnist.py
mpirun --hostfile hostfile -n 6 python3 distributed_gene_analysis.py
```

---

### **Task 2: Hybrid HPC + Big Data Cluster (Docker Swarm + Spark)**

#### Objectives:
- Initialize a Docker Swarm cluster.
- Deploy an Apache Spark stack using Docker Compose.
- Execute distributed ML jobs using PySpark on the same gene expression dataset.

#### Setup Instructions:
1. Initialize Docker Swarm on the master node:

```bash
docker swarm init --advertise-addr <MASTER_IP>
```

2. Join worker nodes using the generated token:

```bash
docker swarm join --token <TOKEN> <MASTER_IP>:2377
```

3. Deploy Spark services using Docker Compose:

```bash
docker stack deploy -c spark-stack.yml spark-cluster
```

4. Access Spark UI at: `http://<MASTER_IP>:8080`

5. Submit PySpark job:

```bash
spark-submit --master spark://<MASTER_IP>:7077 distributed_gene_expression_analysis.py
```

---

## 📊 Results Summary

| Metric                | MPI Cluster        | Spark Cluster         |
|-----------------------|--------------------|------------------------|
| Training Time         | Fast (~0.05–0.6s) | Slower (~15–23s)       |
| Test Accuracy         | ~63–70%           | ~67–70%                |
| Scalability           | Manual scaling     | Auto-scalable via Swarm|
| Fault Tolerance       | Low                | High                   |
| Ease of Monitoring    | Terminal output    | Web UI available       |

---

## 🔧 Troubleshooting Notes

- **SSH Authentication Error**:  
  Issue: `"Authorization required, but no authorization protocol specified"`  
  Fix: Edit `/etc/ssh/sshd_config`, set `X11Forwarding no`, then restart SSH service:

  ```bash
  sudo systemctl restart ssh
  ```

---

## 📎 Documentation

All documentation including:
- Setup logs
- Screenshots
- Final academic report  
are included in the root directory and `screenshots/` folder.

---

## Conclusion

This project successfully demonstrates the integration of traditional HPC techniques with modern Big Data frameworks. Students gained valuable experience in:
- Cluster configuration and networking
- Distributed computing with MPI and Spark
- Orchestration using Docker Swarm
- Bioinformatics data analysis
- System administration and troubleshooting

