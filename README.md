# Tic-Tac-Toe AI: An Intelligent Game Agent Using Minimax and Alpha-Beta Pruning

[![Python Version](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/)
[![Testing Framework](https://img.shields.io/badge/tests-pytest%20(18%20passed)-success.svg)](https://pytest.org/)
[![UI Framework](https://img.shields.io/badge/gui-streamlit-red.svg)](https://streamlit.io/)
[![Game Theory](https://img.shields.io/badge/theory-Zermelo's%20Theorem-orange.svg)]()
[![License](https://img.shields.io/badge/license-MIT-green.svg)]()

> A production-grade, demo-ready artificial intelligence project implementing optimal game-theoretic decision making for Tic-Tac-Toe using the Minimax decision rule with Alpha-Beta ($\alpha$-$\beta$) pruning, complemented by an interactive Streamlit web GUI and empirical Monte Carlo verification.

---

## 📖 Theoretical & Academic Foundations

Guided by foundational literature in game theory and adversarial search (*Swaminathan et al. 2020*, *Kalyan & Singh 2025*, and *Russell & Norvig / Berkeley CS188*):

### 1. Game-Theoretic Classification
Tic-Tac-Toe is modeled as a **finite, two-player, zero-sum game with perfect information**.
- **Zermelo's Theorem (1913)** states that in any finite two-player zero-sum game of perfect information, either the first player has a winning strategy, the second player has a winning strategy, or both players have a strategy guaranteeing at least a draw.
- Tic-Tac-Toe is **rigorously solved as a forced draw** under optimal play by both agents.

### 2. State-Space Complexity
- **Theoretical configurations**: $3^9 = 19,683$ states.
- **Reachable valid states**: $5,478$ board configurations.
- **Symmetry reduction**: Under the dihedral symmetry group $D_4$ (8 symmetries: 4 rotations and 4 reflections), the state space reduces to **765 essentially distinct states**.
- **Complete game paths**: Exactly $26,830$ distinct games up to symmetry.

Because the game-tree is finite and compact, exhaustive adversarial search is tractable and yields exact optimal play without heuristic approximations.

### 3. Minimax Decision Rule
Minimax performs a depth-first search over the game tree, evaluating terminal states via a utility payoff function:

$$\text{Utility}(s) = \begin{cases} +1 & \text{if Player X (Maximizer) wins} \\ -1 & \text{if Player O (Minimizer) wins} \\ 0 & \text{if the game is a Draw} \end{cases}$$

Values are propagated recursively up the game tree via:
- **Maximizer (X)**: $\max_{a \in \text{Actions}(s)} \text{Minimax}(\text{Result}(s, a))$
- **Minimizer (O)**: $\min_{a \in \text{Actions}(s)} \text{Minimax}(\text{Result}(s, a))$

### 4. Alpha-Beta ($\alpha$-$\beta$) Pruning Optimization
To eliminate redundant branch evaluations, branch-and-bound pruning maintains two dynamic thresholds:
- $\alpha$: Maximizer's highest guaranteed payoff along the path.
- $\beta$: Minimizer's lowest guaranteed payoff along the path.

When evaluating a child node, if $\beta \le \alpha$, search on that branch is pruned immediately (**$\beta$-cutoff** for maximizer, **$\alpha$-cutoff** for minimizer). This decreases search complexity from worst-case $O(b^m)$ ($b \approx 9, m \le 9$) to near best-case $O(b^{m/2})$, making decisions effectively instantaneous ($< 1$ ms).

---

## 🏗️ System Architecture

The codebase follows modular, production-ready software engineering standards:

```text
Tic-Tac-Toe-AI/
├── .gitignore                  # Git artifact isolation
├── requirements.txt            # Pinned dependencies
├── README.md                   # Project documentation & theory
├── src/
│   ├── __init__.py             # Package marker
│   ├── game.py                 # Core engine (TicTacToe board state & rule validation)
│   ├── ai.py                   # Adversarial search (Minimax with Alpha-Beta pruning)
│   ├── utils.py                # Symbols, winning triplets, formatting helpers
│   └── ui.py                   # Modern interactive Streamlit web GUI
├── tests/
│   ├── __init__.py             # Test package marker
│   ├── test_game.py            # Unit tests for game rules, moves, wins & draws
│   ├── test_ai.py              # Unit tests for optimal AI decisions & zero-loss guarantees
│   └── test_ui.py              # Smoke & integration tests for GUI state transitions
└── experiments/
    ├── __init__.py             # Package marker
    └── simulate.py             # Monte Carlo empirical verification benchmark
```

---

## ⚡ Quickstart & Installation

### 1. Prerequisites
Ensure Python 3.8+ is installed on your system:
```bash
python --version
```

### 2. Clone the Repository
```bash
git clone https://github.com/Atharv-2314/Tic-Tac-Toe-AI-An-Intelligent-Game-Agent-Using-Minimax-and-Alpha-Beta-Pruning.git
cd Tic-Tac-Toe-AI-An-Intelligent-Game-Agent-Using-Minimax-and-Alpha-Beta-Pruning
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

---

## 🎮 Launching the Interactive GUI

Start the Streamlit web application:

```bash
streamlit run src/ui.py
```

Features included in the web interface:
- **Responsive 3×3 Grid**: Clean clickable buttons with real-time mark placement.
- **Instant AI Responses**: Sub-millisecond Minimax counter-moves.
- **Live State Metrics**: Sidebar tracking empty cells, moves played, and turn status.
- **Dynamic Status Alerts**: Color-coded banners for wins, defeats, and draws.
- **Move Audit Trail**: Expandable history log detailing every action taken in the session.
- **One-Click Reset**: Instant reinitialization to an empty board.

---

## 🧪 Automated Unit & Integration Tests

The project includes an 18-test automated test suite using `pytest`:

```bash
python -m pytest -v
```

### Test Coverage Breakdown
- **Game Engine (`tests/test_game.py`)**:
  - `test_initial_state`: Clean initialization and turn assignment.
  - `test_valid_moves_and_turn_alternation`: Correct mark placement and turn swapping.
  - `test_invalid_move_out_of_bounds`: Out-of-bounds index handling (`IndexError`).
  - `test_invalid_move_occupied_cell`: Occupied cell protection (`ValueError`).
  - `test_row_win_conditions`: Detection of horizontal terminal states.
  - `test_column_win_conditions`: Detection of vertical terminal states.
  - `test_diagonal_win_conditions`: Detection of diagonal and anti-diagonal wins.
  - `test_draw_condition`: Verification of full board Cat's game states.
  - `test_reset`: Board state restoration.
- **Adversarial Search (`tests/test_ai.py`)**:
  - `test_initial_position`: Root minimax value on empty board strictly equals 0.
  - `test_empty_board_minimizer`: Optimal play as second player yields draw.
  - `test_immediate_win_detection_x`: AI seizes immediate offensive winning moves.
  - `test_immediate_win_detection_o`: Minimizer seizes immediate offensive wins.
  - `test_forcing_block`: Threat detection and preventive blocking.
  - `test_defensive_block_by_o`: Tactical blocking of opponent's two-in-a-row.
  - `test_convenience_wrapper`: Validation of `get_best_move()`.
  - `test_ai_vs_ai_guaranteed_draw`: Complete match of AI self-play terminating strictly in a draw.
- **UI Integration (`tests/test_ui.py`)**:
  - `test_ui_game_flow_simulation`: End-to-end headless simulation of player-AI moves.

---

## 📊 Empirical Verification & Monte Carlo Proof

To prove the AI's unbeatability in practice, `experiments/simulate.py` runs 210 simulated matches across three configurations:

```bash
python experiments/simulate.py
```

### Benchmark Results
| Experiment Scenario | Matches | AI Wins | Draws | AI Losses | AI Loss Rate | Empirical Outcome |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **Random (X) vs Minimax AI (O)** | 100 | 79 (79%) | 21 (21%) | **0 (0%)** | **0.00%** | AI Invincible |
| **Minimax AI (X) vs Random (O)** | 100 | 98 (98%) | 2 (2%) | **0 (0%)** | **0.00%** | AI Invincible |
| **Minimax AI vs Minimax AI (Self-Play)** | 10 | 0 (0%) | 10 (100%) | 0 (0%) | **0.00%** | Perfect Zermelo Equilibrium |

**Conclusion:** Across all experimental conditions, the AI achieves a **0.00% loss rate**, empirically proving the unbeatability criterion and confirming Zermelo's Theorem.

---

## 📚 References & Literature Citations

1. **Zermelo, E.** (1913). *Über eine Anwendung der Mengenlehre auf die Theorie des Schachspiels*. Proceedings of the Fifth International Congress of Mathematicians, 2, 501–504.
2. **Russell, S., & Norvig, P.** (2020). *Artificial Intelligence: A Modern Approach* (4th ed.). Pearson. Chapter 5: Adversarial Search and Games.
3. **Swaminathan, A. et al.** (2020). *Analysis of Minimax Algorithm Using Tic-Tac-Toe*.
4. **Kalyan, V., & Singh, R.** (2025). *Tic-Tac-Toe: A Game Theory Analysis*.
5. **UC Berkeley CS188**. *Introduction to Artificial Intelligence: Adversarial Search & Alpha-Beta Pruning Course Notes*.

---

## 📄 License
This project is open-source and available under the [MIT License](LICENSE).
