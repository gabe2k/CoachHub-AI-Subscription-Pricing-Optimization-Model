CoachHub AI – Subscription Pricing Optimization Model

This project builds a Python-based pricing simulation model for CoachHub AI, a SaaS platform for fitness coaches that provides a Coach Dashboard, AI-generated training programs, AI-assisted check-ins, and client analytics.

The goal of the model is to:

* Determine profit-maximizing price points for three subscription tiers
* Model willingness-to-pay (WTP), churn, and lifetime value (LTV) for different coach segments
* Provide a framework for future A/B tests, experiments, and go-to-market pricing strategy

---

Tier Structure

CoachHub AI offers three subscription tiers aligned with typical coach client-loads and feature needs:

Basic — 1 to 3 clients

* AI-only program generation based on client intake

Standard — 4 to 10 clients

* AI programming + AI-assisted weekly check-ins

Premium — 11+ clients

* AI programming
* Weekly check-ins
* Client analytics + AI-generated weekly summaries

Each tier includes:

* A maximum number of client slots
* A segment-specific base demand (coaches in-market for that tier)
* A WTP distribution (mean and standard deviation)
* A baseline churn probability
* An estimated cost-to-serve per client

These assumptions are stored in data/coachhub_tiers.csv.

---

Data

File: data/coachhub_tiers.csv

Columns:

* tier (Basic / Standard / Premium)
* max_clients (maximum clients supported on the tier)
* base_demand (potential demand for the tier)
* wtp_mean (average willingness-to-pay per month)
* wtp_std (variation in willingness-to-pay)
* cost_per_client (estimated infra + compute + AI cost)
* base_churn (baseline monthly churn rate)
* feature_sensitivity (sensitivity to advanced features)

These values are synthetic but chosen to be realistic for an early-stage SaaS fitness platform.

---

Modeling Approach

1. Willingness-to-Pay (WTP) Simulation

   * The model samples thousands of hypothetical coaches from a normal WTP distribution
   * At each price, conversion rate = percentage of coaches whose WTP is greater than or equal to the price
   * Demand = base_demand × conversion_rate

2. Churn as a Function of Price

   * If price exceeds WTP expectations, churn increases proportionally
   * This captures the trade-off between maximizing revenue and retaining coaches

3. Lifetime Value (LTV) and Profit Calculation

   * Expected lifetime in months is approximated as 1 / churn_rate
   * lifetime_revenue = demand × price × expected_months
   * lifetime_cost    = demand × max_clients × cost_per_client × expected_months
   * profit           = lifetime_revenue – lifetime_cost

4. Optimization

   * For each tier, the model sweeps across a price range:
     Basic: $10–60
     Standard: $40–120
     Premium: $80–200
   * It selects the price that maximizes lifetime profit for that tier

---

Recommended Price Points (Example Results)

Based on current assumptions:

Basic: $29/month
Standard: $79/month
Premium: $149/month

These recommendations are:

* Accessible for early-stage and scaling coaches
* Competitive with Trainerize, TrueCoach, and Everfit
* Justified by the hours saved through AI programming and check-ins

---

Installation

git clone [https://github.com/](https://github.com/)<your-username>/coachhub-pricing-model.git
cd coachhub-pricing-model

python -m venv .venv
source .venv/bin/activate    # Windows: .venv\Scripts\activate
pip install -r requirements.txt
