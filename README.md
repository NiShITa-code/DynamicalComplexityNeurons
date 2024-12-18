# DynamicalComplexityNeurons

## 1. Introduction  
This project involves simulating small-world modular networks of Izhikevich neurons. The modular structure includes excitatory and inhibitory neurons, with network connectivity adjusted to study the effects of different rewiring probabilities on network dynamics. Poisson noise is introduced to simulate background firing activity. Various plotting techniques are used to visualize connectivity, neuron firing (raster plot), and mean firing rate over time.

---

## 1.2 Overview  
The solution comprises the following Python file:  
- **Modular.py**: Simulates the modular neuron network following the Dynamical Complexity Experiment and generates the necessary plots.  

---

## 1.3 Structure of the Python Code  

The Python code is encapsulated within the `ModularNetwork` class, which inherits from the `IzNetwork` class (defined in `iznetwork.py`). It extends the functionality to handle modular, small-world networks with excitatory and inhibitory neurons.  

### 1.3.1 Methods in `ModularNetwork`  

- **`__init__(self, N_excitatory=800, N_inhibitory=200, Dmax=20)`**  
  - Initializes a modular network with the specified number of excitatory and inhibitory neurons.  
  - Sets the maximum conduction delay for synaptic connections (`Dmax`).  
  - Calls methods to create initial connections and set neuron parameters.  

- **`create_initial_connections(self)`**  
  - Divides the network into multiple modules and establishes connections within each module.  
  - Initializes weights (`self.W`), scaling, and delays (`self.D`) for the network, setting up initial connectivity.  

    - **Excitatory-to-Excitatory (within modules):**  
      - Each module contains 100 excitatory neurons, and 1000 directed random connections are established between them.  
    - **Excitatory-to-Inhibitory:**  
      - Each inhibitory neuron receives connections from four excitatory neurons within the same module.  
    - **Inhibitory-to-Excitatory and Inhibitory-to-Inhibitory (Diffuse Inhibition):**  
      - Each inhibitory neuron projects to all other neurons in the network, providing diffuse inhibition.  

- **`rewire_network(self, p)`**  
  - Rewires the existing network connections with a given probability `p`.  
  - Ensures connections are reassigned to new target neurons in other modules without forming self-connections or duplicate connections.  

- **`set_neuron_parameters(self)`**  
  - Assigns parameters (`a, b, c, d`) with randomization parameter `r` for each neuron based on whether it is excitatory or inhibitory.  
  - These parameters control neuron behavior based on Izhikevich’s spiking neuron model, imported from `iznetwork.py`.  

- **`run_simulation(self, duration_ms, p)`**  
  - Runs the network simulation for the specified duration (`duration_ms`).  
  - Allows rewiring connections with probability `p` before the main simulation.  
  - Introduces Poisson noise to simulate spontaneous background activity.  
  - Includes a transient period (100ms) at the start of the simulation to allow the network to stabilize.  
  - Records firing data throughout the simulation.  

- **Plotting Functions:**  
  - **`plot_connectivity_matrix(self)`**  
    - Visualizes the connectivity matrix of the modular network.  
  - **`plot_raster(self, firings, T)`**  
    - Generates a raster plot to visualize neuron firing over time.  
  - **`plot_mean_firing_rate(self, firings, T)`**  
    - Plots the mean firing rate of neurons over time, averaged across different modules.  

---

## 1.4 How to Run the Code  

### Dependencies (Requirements)  
- Python 3.x  
- `numpy`: For numerical operations and matrix manipulations.  
- `matplotlib`: For generating plots such as connectivity matrices, raster plots, and mean firing rate visualizations.  
- `iznetwork.py`: The base class for neuron networks must be available in Python’s import path.  

### Usage (Running the code)  
To run the simulation, instantiate the `ModularNetwork` class as follows:  

```python
if __name__ == "__main__":
    network = ModularNetwork()
    p = [0, 0.1, 0.2, 0.3, 0.4, 0.5]
    for i in p:
        firings = network.run_simulation(1000, i)
        network.plot_raster(firings, i)
        network.plot_mean_firing_rate(firings, 1000, i)
        network.plot_connectivity_matrix(i)
