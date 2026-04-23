\# 🚀 Kubernetes Priority \& Preemption Demo (kind)



\## 📌 Project Overview



This project demonstrates how \*\*Kubernetes Pod Priority and Preemption\*\* works under resource pressure.



You will see:



\* How Kubernetes schedules Pods based on priority

\* How high-priority Pods \*\*evict lower-priority Pods\*\*

\* Real-time scheduler behavior using events and logs



\---



\## 🧠 Key Concepts



\### 🔹 Priority



Each Pod is assigned a priority using a `PriorityClass`.

Higher priority Pods are scheduled before lower priority ones.



\### 🔹 Preemption



When the cluster is full:



\* A high-priority Pod cannot be scheduled ❌

\* Kubernetes \*\*evicts (kills)\*\* lower-priority Pods

\* Frees resources → schedules high-priority Pod ✅



\---



\## 📂 Project Structure



```

k8s-preemption-demo/

&#x20;├── namespace.yaml

&#x20;├── priority-classes.yaml

&#x20;├── low-priority-deploy.yaml

&#x20;├── high-priority-pod.yaml

&#x20;└── README.md

```



\---



\## ⚙️ Prerequisites



\* kind installed

\* kubectl installed

\* Docker running



\---



\## 🏗️ Step 1: Create Cluster



```bash

kind create cluster --name preemption-demo

```



\---



\## 📦 Step 2: Apply Resources



```bash

kubectl apply -f namespace.yaml

kubectl apply -f priority-classes.yaml

kubectl apply -f low-priority-deploy.yaml

```



\---



\## 📊 Step 3: Verify Low Priority Pods



```bash

kubectl get pods -n preemption-lab

```



Expected:



\* All low-priority pods in \*\*Running\*\* state



\---



\## 👀 Step 4: Watch Pods (IMPORTANT)



```bash

kubectl get pods -n preemption-lab -w

```



\---



\## 💥 Step 5: Trigger Preemption



```bash

kubectl apply -f high-priority-pod.yaml

```



\---



\## 📸 Screenshots



All command outputs and observations are captured in the \*\*\[screenshots/](./screenshots)\*\* folder.



\---



\## 🔍 What Happened Internally



1\. Low-priority Pods consumed most CPU

2\. High-priority Pod was created

3\. Scheduler failed → insufficient CPU

4\. Preemption triggered

5\. Low-priority Pods evicted

6\. High-priority Pod scheduled



\---



\## 🔥 Advanced Scenario: Disable Preemption



Modify high-priority pod:



```yaml

preemptionPolicy: Never

```



\### Result:



\* Pod stays \*\*Pending\*\*

\* No eviction happens



👉 Shows that preemption is \*\*configurable\*\*



\---



\## ⚠️ Troubleshooting



\### ❌ Preemption not happening?



\* Increase CPU requests

\* Increase replicas of low-priority Pods

\* Ensure all Pods run on same node



\---



\## 💡 Key Learnings



\* Preemption only happens under \*\*resource pressure\*\*

\* Scheduler waits for terminating Pods before scheduling

\* Priority does not guarantee immediate scheduling

\* Preemption can be disabled



\---



\## 🏁 Conclusion



This project demonstrates how Kubernetes ensures that \*\*critical workloads are never starved\*\*, by dynamically reallocating resources using priority and preemption.



\---



\## ⭐ Use Case



\* Production workloads vs batch jobs

\* Multi-tenant clusters

\* Critical system components (monitoring, logging)



\---



\## 🙌 Author



Kiran Kumatkar



