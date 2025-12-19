# Battery-Thermal-Risk-Assessment-Using-Data-Driven-Markov-Models
 
A Continuous-Time Markov Chain (CTMC) model for predicting thermal runaway in battery systems using temperature sensor data. This project implements a state-based predictive model that assesses thermal risk and provides early warning indicators.
 
## 📋 Overview
 
This project uses CTMC modeling to predict the probability of thermal runaway in battery systems by:
- Analyzing temperature data from multiple thermocouples
- Classifying system states based on maximum temperature thresholds
- Computing transition rates between states
- Predicting future state probabilities
- Calculating a Thermal Risk Index (TRI) for risk assessment
 
## 🔬 Features
 
- **State Classification**: Automatically assigns states (S0, S1, S2, S3) based on temperature thresholds
- **CTMC Model Training**: Learns transition rates from historical data
- **Probability Prediction**: Forecasts future state probabilities using matrix exponentiation
- **Thermal Risk Index (TRI)**: Computes a weighted risk score for thermal runaway assessment
- **Real-time Simulation**: Simulates battery behavior with configurable parameters
- **Visualization**: Generates plots for temperature, TRI, and runaway probability
 
## 📁 Project Structure
 
```
.
├── train.py                      # Model training script
├── simulate.py                   # Basic simulation script
├── ctmc_simulate_with_output.py  # Advanced simulation with output and plots
├── ctmc_model.py                 # CTMC model implementation
├── preprocess.py                 # Data preprocessing utilities
├── requirements.txt              # Python dependencies
├── train.csv                     # Training dataset
├── validate.csv                  # Validation dataset
├── simulate.csv                  # Simulation dataset
├── ctmc_model.pkl               # Trained model (generated)
└── ctmc_output.csv              # Simulation output (generated)
```
 
## 🚀 Installation
 
### Prerequisites
 
- Python 3.7 or higher
- pip package manager
 
### Setup
 
1. Clone the repository:
```bash
git clone <repository-url>
cd Master
```
 
2. Install dependencies:
```bash
pip install -r requirements.txt
```
 
## 📊 Usage
 
### 1. Training the Model
 
Train the CTMC model using training and validation datasets:
 
```bash
python train.py
```
 
This will:
- Load training and validation data
- Compute transition counts and time spent in each state
- Build the rate matrix Q
- Save the trained model to `ctmc_model.pkl`
 
### 2. Basic Simulation
 
Run a simple simulation to predict probabilities and TRI:
 
```bash
python simulate.py
```
 
### 3. Advanced Simulation with Output
 
Run a comprehensive simulation that generates CSV output and visualization plots:
 
```bash
python ctmc_simulate_with_output.py
```
 
With custom parameters:
```bash
python ctmc_simulate_with_output.py --simulate simulate.csv --model ctmc_model.pkl --output results.csv --speed 0.1
```
 
**Parameters:**
- `--simulate`: Path to simulation CSV file (default: `simulate.csv`)
- `--model`: Path to trained model file (default: `ctmc_model.pkl`)
- `--output`: Output CSV file path (default: `ctmc_output.csv`)
- `--speed`: Delay per row in seconds (default: 0.2)
 
## 🔍 State Definitions
 
The model uses four states based on maximum temperature:
 
| State | Temperature Range | Description |
|-------|------------------|-------------|
| **S0** | < 17°C | Normal/Low temperature |
| **S1** | 17-25°C | Elevated temperature |
| **S2** | 25-40°C | High temperature |
| **S3** | ≥ 40°C | Thermal runaway (absorbing state) |
 
## 📈 Output
 
### CSV Output
 
The simulation generates a CSV file with the following columns:
- `Time[s]`: Timestamp in seconds
- `State`: Current state (0-3)
- `P_S0`, `P_S1`, `P_S2`, `P_S3`: Predicted probabilities for each state
- `TRI`: Thermal Risk Index
- `STATUS`: Risk status (NORMAL, CAUTION, HIGH RISK)
- `MAX_TEMP`: Maximum temperature at current time
 
### Visualization Plots
 
The simulation automatically generates three plots:
1. **plot_max_temp.png**: Maximum temperature over time
2. **plot_tri.png**: Thermal Risk Index over time
3. **plot_s3_probability.png**: Probability of thermal runaway (S3) over time
 
## 🧮 Model Details
 
### Rate Matrix Construction
 
The rate matrix Q is built using maximum likelihood estimation:
- Transition rates: `q_ij = N_ij / T_i`
  - `N_ij`: Number of transitions from state i to state j
  - `T_i`: Total time spent in state i
- Diagonal elements: `q_ii = -Σ(q_ij)` for j ≠ i
- S3 is an absorbing state (no transitions out)
 
### Probability Prediction
 
Future state probabilities are computed using matrix exponentiation:
```
P(t) = P(0) × exp(Q × t)
```
 
### Thermal Risk Index (TRI)
 
TRI is calculated as a weighted sum of state probabilities:
```
TRI = 0.0 × P(S0) + 0.3 × P(S1) + 0.7 × P(S2) + 1.0 × P(S3)
```
 
**Risk Thresholds:**
- TRI < 0.2: **NORMAL**
- 0.2 ≤ TRI < 0.5: **CAUTION**
- TRI ≥ 0.5: **HIGH RISK** (possible thermal runaway)
 
## 📝 Data Format
 
Input CSV files should contain the following columns:
- `Test Time [s]`: Timestamp in seconds
- `TC1 near positive terminal [C]`: Temperature sensor 1
- `TC2 near negative terminal [C]`: Temperature sensor 2
- `TC3 bottom - bottom [C]`: Temperature sensor 3
- `TC4 bottom - top [C]`: Temperature sensor 4
- `TC5 above punch [C]`: Temperature sensor 5
- `TC6 below punch [C]`: Temperature sensor 6
 
The preprocessing automatically computes `MAX_TEMP` and assigns states.
 
## 🛠️ Dependencies
 
- **pandas**: Data manipulation and CSV handling
- **numpy**: Numerical computations
- **scipy**: Matrix exponentiation (`scipy.linalg.expm`)
- **matplotlib**: Plot generation (for simulation script)
 
## 📄 License
 
[Specify your license here]
 
## 👥 Authors
 
[Add author information]
 
## 🤝 Contributing
 
Contributions are welcome! Please feel free to submit a Pull Request.
 
## 📧 Contact
 
[Add contact information]
 
---
 
**Note**: This is a research/educational project for thermal runaway prediction. Always follow proper safety protocols when working with battery systems.
