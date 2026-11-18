# Datasets exposed during development

Each dataset below contributed labeled query rows from the first TabArena outer test split to the development score. We also evaluated both candidate checkpoints on TabArena-Lite before selecting 1,024 for submission, so all 51 first splits were seen at final selection. Bioresponse, QSAR-TID-11 and hiva_agnostic were outside this 48-table panel.

- APSFailure
- Amazon_employee_access
- Another-Dataset-on-used-Fiat-500
- Bank_Customer_Churn
- Diabetes130US
- E-CommereShippingData
- Fitness_Club
- Food_Delivery_Time
- GiveMeSomeCredit
- HR_Analytics_Job_Change_of_Data_Scientists
- Is-this-a-good-customer
- MIC
- Marketing_Campaign
- NATICUSdroid
- QSAR_fish_toxicity
- SDSS17
- airfoil_self_noise
- anneal
- bank-marketing
- blood-transfusion-service-center
- churn
- coil2000_insurance_policies
- concrete_compressive_strength
- credit-g
- credit_card_clients_default
- customer_satisfaction_in_airline
- diabetes
- diamonds
- hazelnut-spread-contaminant-detection
- healthcare_insurance_expenses
- heloc
- houses
- in_vehicle_coupon_recommendation
- jm1
- kddcup09_appetency
- maternal_health_risk
- miami_housing
- online_shoppers_intention
- physiochemical_protein
- polish_companies_bankruptcy
- qsar-biodeg
- seismic-bumps
- splice
- students_dropout_and_academic_success
- superconductivity
- taiwanese_bankruptcy_prediction
- website_phishing
- wine_quality

We evaluated the 2,048 support cap after the 512-support baseline was seen, based on prior development-panel evidence. The submitted default matches the 200,000-row Kumo Small cap and has no local full Lite run. The new Lite run reuses the same 51 outer test splits.
