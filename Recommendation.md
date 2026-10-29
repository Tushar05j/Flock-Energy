# Practical Implications and Recommendations

## 1. Energy-Saving Opportunity

The analysis suggests an opportunity to improve household energy awareness by estimating washing-machine usage from aggregate electricity readings. A smart energy-monitoring system could identify likely washing-machine operating periods and help users understand their consumption patterns. Where time-of-use electricity tariffs are available, users could shift flexible washing cycles to lower-tariff periods. The effectiveness of this approach could be evaluated by comparing electricity consumption and costs before and after changing usage schedules.

## 2. Limitations and Generalizability

Appliance disaggregation has limitations because different appliances can have similar power signatures, particularly when multiple devices operate simultaneously. Missing readings and long outages can also reduce data quality and model accuracy. Furthermore, household consumption patterns vary, so a model trained on UK REFIT households may not generalize directly to Indian households because of differences in appliances, usage habits, and electricity tariffs. Testing on additional households and local datasets would be necessary before practical deployment.

## 3. Effect of Lower-Resolution Smart-Meter Data

If only 15-minute or 30-minute smart-meter readings were available instead of one-minute readings, short washing-machine cycles and rapid changes in power consumption would be harder to identify. Averaging readings over longer intervals can hide brief power peaks and combine the activity of multiple appliances into a single measurement. Consequently, appliance detection, accurate power estimation, and identification of overlapping loads would become more difficult. The model would need to be adapted and evaluated at the lower resolution, and its accuracy could decrease.
