 Quantum Simulation of the Water Molecule ($\text{H}_2\text{O}$)

This repository contains a Jupyter notebook/Python script that performs a Variational Quantum Eigensolver (VQE) simulation to calculate the ground state energy of the water molecule ($\text{H}_2\text{O}$). This project serves as a practical demonstration of quantum chemistry algorithms run on a quantum computer simulator (Qiskit Aer).

Quantum Physics and Chemistry Background

The core challenge addressed here is solving the electronic Schrödinger equation for the water molecule to determine its lowest energy state (ground state).

1. The Electronic Structure Problem

The Hamiltonian ($\hat{H}$) describes the total energy of the system. For a molecule, the Hamiltonian includes:

Kinetic energy of electrons.

Potential energy from electron-nucleus attraction.

Potential energy from electron-electron repulsion.

Potential energy from nucleus-nucleus repulsion (treated classically here).

Solving $\hat{H}\Psi = E\Psi$ exactly for molecules is impossible for classical computers due to the exponential growth of the required computational space (Hilbert space). Quantum algorithms, like VQE, offer a path to approximate these solutions efficiently.

2. Qubit Encoding: Mapping to a Quantum Processor

To run on a quantum circuit, the chemical problem (expressed in second quantization using fermionic operators) must be translated into a qubit language (using Pauli operators).

Fermionic to Qubit Mapping: We use the Jordan-Wigner Transformation  to convert the second-quantized Hamiltonian into a qubit Hamiltonian ($\hat{H}_{\text{qubit}}$), which is a linear combination of tensor products of Pauli matrices ($\text{I}, \text{X}, \text{Y}, \text{Z}$).

$$\\ \hat{H}*{\text{elec}} \longrightarrow \hat{H}*{\text{qubit}} = \sum\_i c\_i P\_i$$

$$$$Where $P_i$ are Pauli strings (e.g., $\text{X} \otimes \text{Y} \otimes \text{I} \otimes \text{Z}$).

3. The Variational Quantum Eigensolver (VQE)

VQE is a hybrid quantum-classical algorithm that leverages the Variational Principle of quantum mechanics.

The Principle: The expected energy calculated from any arbitrary trial wavefunction ($\Psi$) will always be greater than or equal to the true ground state energy ($E_0$).

$$\\ E(\vec{\theta}) = \frac{\langle \Psi(\vec{\theta}) | \hat{H}\_{\text{qubit}} | \Psi(\vec{\theta}) \rangle}{\langle \Psi(\vec{\theta}) | \Psi(\vec{\theta}) \rangle} \geq E\_0$$

$$$$

VQE Workflow:

Preparation (Quantum): A parameterized quantum circuit (the Ansatz) is constructed. The H2O.ipynb uses the Unitary Coupled Cluster Singles and Doubles (UCCSD) Ansatz, a highly accurate and chemically inspired circuit tailored for molecular systems.

Measurement (Quantum): The quantum computer (or simulator) executes the Ansatz and measures the expectation value of $\hat{H}_{\text{qubit}}$ for the current parameters ($\vec{\theta}$).

Optimization (Classical): A classical optimizer (SLSQP in this case) receives the energy value $E(\vec{\theta})$ and suggests new, adjusted parameters ($\vec{\theta}_{\text{new}}$) to lower the energy.

This loop repeats, iteratively driving the energy towards the true quantum mechanical ground state minimum.

Simulation Details

Component

Quantum Significance

Value/Technique Used

Molecule Geometry

Defines the fixed nuclear potential.

$\text{O}(0,0,0); \text{H}(\pm 0.757, 0.586, 0)$

Basis Set

Dictates the dimensionality of the electronic problem (number of molecular orbitals).

STO-3G (Minimal Basis Set)

Mapping

Bridge between chemistry (fermions) and quantum computing (qubits).

JordanWignerMapper

Initial State

Provides a reasonable starting point for the VQE optimization.

HartreeFock state

Ansatz

The quantum circuit structure used to prepare the trial wavefunction $\Psi(\vec{\theta})$.

UCCSD

Optimizer

Classical routine to find the optimal parameters $\vec{\theta}$.

SLSQP (Sequential Least Squares Programming)

Estimator

Backend for efficiently calculating the expectation value $\langle\hat{H}_{\text{qubit}}\rangle$.

AerEstimator (Qiskit Simulator)

Getting Started

Prerequisites

You need Python and the necessary quantum and scientific computing libraries.

pip install qiskit qiskit-nature qiskit-aer numpy matplotlib pyscf


Running the Simulation

Clone the repository (if applicable) or ensure H2O.ipynb is accessible.

Execute the Python script/notebook:

# If run as a Python script
python H2O.ipynb 

# If run in a Jupyter/VS Code environment
jupyter notebook H2O.ipynb 
# (Then run all cells)


Expected Output

The program prints the energy calculation at each optimization step (as shown in the excerpt below) and generates a convergence plot showing the energy trace decreasing towards the converged ground state value.

Executing VQE algorithm...

Iteration:   0 | Energy: -84.164242824108
Iteration:   1 | Energy: -84.173264777901
...
VQE Simulation Complete
Final Ground State Energy: -84.147457733336 Hartrees 
