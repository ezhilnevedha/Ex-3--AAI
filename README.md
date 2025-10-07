<H3>ENTER YOUR NAME: EZHIL NEVEDHA K</H3>
<H3>ENTER YOUR REGISTER NO.212223230055 </H3>
<H3>EX. NO.3</H3>
<H3>DATE:07.10.2025 </H3>
<H1 ALIGN =CENTER> Implementation of Approximate Inference in Bayesian Networks
</H1>

## Aim: 
   To construct a python program to implement approximate inference using Gibbs Sampling.</br>
## Algorithm:
   Step 1: Bayesian Network Definition and CPDs:<br>
    <ul> <li>Define the Bayesian network structure using the BayesianNetwork class from pgmpy.models.</li>
    <li>Define Conditional Probability Distributions (CPDs) for each variable using the TabularCPD class.</li>
    <li>Add the CPDs to the network.</li></ul>
    Step 2: Printing Bayesian Network Structure:<br>
    <ul><li>Print the structure of the Bayesian network using the print(network) statement.</li></ul>
   Step 3: Graph Visualization:
    <ul><li>Import the necessary libraries (networkx and matplotlib).</li>
    <li>Create a directed graph using networkx.DiGraph().</li>
    <li>Define the nodes and edges of the graph.</li>
    <li>Add nodes and edges to the graph.</li>
    <li>Optionally, define positions for the nodes.</li>
    <li>Use nx.draw() to visualize the graph using matplotlib.</li></ul>
    Step 4: Gibbs Sampling and MCMC:<br>
    <ul><li>Initialize Gibbs Sampling for MCMC using the GibbsSampling class and provide the Bayesian network.</li>
    <li>Set the number of samples to be generated using num_samples.</li></ul>
    Step 5: Perform MCMC Sampling:<br>
    <ul><li>Use the sample() method of the GibbsSampling instance to perform MCMC sampling.</li>
    <li>Store the generated samples in the samples variable.</li></ul>
    Step 6: Approximate Probability Calculation:<br>
    <ul><li>Specify the variable for which you want to calculate the approximate probabilities (query_variable).</li>
    <li>Use .value_counts(normalize=True) on the samples of the query_variable to calculate approximate probabilities.</li></ul>
    Step 7:Print Approximate Probabilities:<br>
    <ul><li>Print the calculated approximate probabilities for the specified query_variable.</li></ul>


## Program:
```
# Import required libraries
import matplotlib.pyplot as plt
import networkx as nx
from pgmpy.models import DiscreteBayesianNetwork
from pgmpy.factors.discrete import TabularCPD
from pgmpy.sampling import GibbsSampling

# -------------------------------
# 1. Define the Bayesian Network
# -------------------------------
alarm_model = DiscreteBayesianNetwork([
    ("Burglary", "Alarm"),
    ("Earthquake", "Alarm"),
    ("Alarm", "JohnCalls"),
    ("Alarm", "MaryCalls"),
])

# -------------------------------
# 2. Define CPDs (Conditional Probability Tables)
# -------------------------------
cpd_burglary = TabularCPD("Burglary", 2, [[0.999], [0.001]])
cpd_earthquake = TabularCPD("Earthquake", 2, [[0.998], [0.002]])
cpd_alarm = TabularCPD(
    "Alarm", 2,
    [[0.999, 0.71, 0.06, 0.05], [0.001, 0.29, 0.94, 0.95]],
    evidence=["Burglary", "Earthquake"],
    evidence_card=[2, 2]
)
cpd_johncalls = TabularCPD(
    "JohnCalls", 2,
    [[0.95, 0.1], [0.05, 0.9]],
    evidence=["Alarm"],
    evidence_card=[2]
)
cpd_marycalls = TabularCPD(
    "MaryCalls", 2,
    [[0.99, 0.3], [0.01, 0.7]],
    evidence=["Alarm"],
    evidence_card=[2]
)

# Add CPDs to the model
alarm_model.add_cpds(cpd_burglary, cpd_earthquake, cpd_alarm, cpd_johncalls, cpd_marycalls)

print("Bayesian Network Structure:")
print(alarm_model)

# -------------------------------
# 3. Draw the Bayesian Network
# -------------------------------
G = nx.DiGraph()
edges = [
    ("Burglary", "Alarm"),
    ("Earthquake", "Alarm"),
    ("Alarm", "JohnCalls"),
    ("Alarm", "MaryCalls")
]
G.add_edges_from(edges)

pos = {
    "Burglary": (0, 0),
    "Earthquake": (2, 0),
    "Alarm": (1, -2),
    "JohnCalls": (0, -4),
    "MaryCalls": (2, -4)
}

fig, ax = plt.subplots(figsize=(6, 6))
nx.draw(
    G, pos,
    with_labels=True,
    node_size=1500,
    node_color="skyblue",
    font_size=10,
    font_weight="bold",
    arrowsize=20,
    ax=ax
)
plt.title("Bayesian Network: Burglar Alarm Problem")
plt.show()

# -------------------------------
# 4. Gibbs Sampling
# -------------------------------
gibbssampler = GibbsSampling(alarm_model)
num_samples = 10000
samples = gibbssampler.sample(size=num_samples)

# -------------------------------
# 5. Approximate Inference
# -------------------------------
# Marginal probability of Burglary
query_variable = "Burglary"
query_result = samples[query_variable].value_counts(normalize=True)

print("\nApproximate probabilities of {}:".format(query_variable))
print(query_result)

# Conditional probability: P(Burglary | JohnCalls=1, MaryCalls=1)
cond_samples = samples[(samples["JohnCalls"] == 1) & (samples["MaryCalls"] == 1)]
cond_result = cond_samples["Burglary"].value_counts(normalize=True)

print("\nApproximate P(Burglary | JohnCalls=1, MaryCalls=1):")
print(cond_result)

```



## Output:
<img width="804" height="654" alt="image" src="https://github.com/user-attachments/assets/d3eec6c6-97e4-4f28-8593-8e91ec169e61" />

<img width="516" height="251" alt="image" src="https://github.com/user-attachments/assets/15310da1-d6ef-4ffb-94be-28699ea8eab0" />

## Result:
Thus, Gibb's Sampling( Approximate Inference method) is succuessfully implemented using python.
