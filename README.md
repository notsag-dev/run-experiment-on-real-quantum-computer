
# Testing the simplest circuit ever on a real quantum computer
Note: This readme file has been generated from the Jupyter notebook's output included in the repository.

The objective of this experiment is to get to execute the simplest circuit ever on an IBM's real quantum computer for free in just a few minutes (isn't that pretty cool, amigo?). Please check [IBMQ](https://quantum-computing.ibm.com/) and [Qiskit](https://qiskit.org) for getting a bit of context if you haven't done so already.

First of all, we need to set up our IBM Q account. Please [register](https://qiskit.org/documentation/install.html#access-ibm-q-systems) and get your API token as shown in the link, paste it below and run:


```python
from qiskit import IBMQ

ibm_api_token = 'YOUR TOKEN HERE'
IBMQ.save_account(ibm_api_token)
IBMQ.load_account()
```

We're now all set to start using real quantum computers! Let's list all the available backends we can use, including most likely several quantum computers and a simulator:


```python
provider = IBMQ.get_provider('ibm-q')

print("List of backends:\n")
for backend in provider.backends():
    print(backend.name())
    props = backend.properties()
    if (props):
        print(f"Qubits:      {len(props.qubits)}")
    else:
        print(f"Simulator")
    print(f"Queued jobs: {backend.status().pending_jobs}\n")
```

    List of backends:
    
    ibmq_qasm_simulator
    Simulator
    Queued jobs: 0
    
    ibmqx2
    Qubits:      5
    Queued jobs: 3
    
    ibmq_16_melbourne
    Qubits:      15
    Queued jobs: 20
    
    ibmq_vigo
    Qubits:      5
    Queued jobs: 17
    
    ibmq_ourense
    Qubits:      5
    Queued jobs: 13
    
    ibmq_london
    Qubits:      5
    Queued jobs: 1879
    
    ibmq_burlington
    Qubits:      5
    Queued jobs: 3
    
    ibmq_essex
    Qubits:      5
    Queued jobs: 4
    
    ibmq_armonk
    Qubits:      1
    Queued jobs: 1
    


From the backend list, select one and replace its name below to use it as your backend. I'm going to go with ibmq_armonk, it's hopefully the fastest option as it has just one queued job and its one qubit will be enough for this experiment.


```python
backend_name = 'ibmq_armonk'
```

Let's now create a very simple circuit with just one qubit, and visualize it:


```python
from qiskit import (QuantumCircuit, execute)
from qiskit.visualization import plot_histogram

n_qubits = 1
n_outputs = 1
circuit = QuantumCircuit(n_qubits, n_outputs)
circuit.measure(0,0)
circuit.draw()
```




<pre style="word-wrap: normal;white-space: pre;background: #fff0;line-height: 1.1;font-family: &quot;Courier New&quot;,Courier,monospace">        ┌─┐
q_0: |0>┤M├
        └╥┘
 c_0: 0 ═╩═
           </pre>



Note that the only thing we do is measuring the value of the qubit. This qubit when measured has a probability = 1 of having state = 0 (by default). Let's see what happens when executing 1000 shots on the selected backend:


```python
provider = IBMQ.get_provider("ibm-q")
backend = provider.get_backend(backend_name)
job = execute(circuit, backend, shots=1000)
job_result = job.result()

print("Results:", job_result.get_counts(circuit))
```

    Results: {'0': 963, '1': 37}


Congratulations! If you reached this point you have already tested your first circuit on a real quantum computer! Please note the noise of our results, while theoretically the qubit had a probability = 1 of having state = 0 when measured, we still observed 37 times state = 1.

## Conclusions
We have created a project that allows us to test a simple quantum cirquit with one qubit and one output on IBM Q cloud. The process was straightforward and the tool is well worth a try because of its huge potential, also being able to work with Python using Qiskit makes it so easy to get started with it. What's not easy, though, is to wrap one's head around all the quantum computing concepts and physical layer issues that are not abstracted here, but it's definitely an amazing exercise!
