# Home Assistant: SigenStor Inverter & Amber Electric Automations

This repository contains Home Assistant YAML automations designed to optimise a Sigenergy SigenStor inverter (9 kW solar array, 16 kWh battery) for Amber Electric's wholesale pricing in Victoria, Australia. 

The logic is tailored for the **Jemena Residential Export (A10E)** network tariff, maximising evening export rewards while actively blocking daytime solar soak penalties.

## System Hardware
* **Inverter and Battery:** Sigenergy SigenStor (16 kWh capacity)
* **Solar Array:** 9 kW
* **Integration:** Local Modbus TCP control via Home Assistant
* **Hot Water:** SPT Heat Pump

## Core Automations

### 1. Amber: Block Negative Exports
This automation dynamically throttles the SigenStor's grid export limit to 0 kW to protect against negative wholesale feed-in tariffs and specific network penalties. The internal MPPT controllers naturally curtail the roof's solar generation to perfectly match the household AC load and battery charging demand.
* **Wholesale Protection:** Clamps exports when the general feed-in price drops below 0c/kWh.
* **Jemena A10E Protection:** Clamps exports when the feed-in price drops below 3.0c/kWh between 11:00 AM and 4:00 PM, perfectly neutralising the Jemena midday solar soak penalty.

### 2. Amber - SPT Heat Pump Energy Optimiser
Instead of continually curtailing excess midday solar generation to 0W, this automation reroutes the surplus energy to heat the household hot water cylinder. It actively soaks up daytime generation, ensuring maximum self-consumption before any remaining power is evaluated for grid export.

### 3. Amber: Summer Spike Exporter
This automation capitalises on the Jemena A10E 15.0c/kWh evening network reward and major wholesale price spikes between 4:00 PM and 9:00 PM.
* **Routine Evening Peak:** Discharges the SigenStor down to a 55% State of Charge (SoC) during standard evening price peaks above 28c/kWh.
* **Major Spike Mode:** Unlocks the battery reserve and discharges deeply down to a 30% SoC floor during major wholesale spikes, stacking the wholesale feed-in credits on top of the Jemena network reward.

## Amber Electric Referral Code

If you are setting up your own hardware on Home Assistant and want to test wholesale energy pricing, you can use my referral link to claim a sign-up bonus.

* **Referral Code:** `PYMY77DY`
* **Application:** Enter `PYMY77DY` in the "Promo Code" or "Referral Code" field during the online sign-up flow on the Amber Electric website.
* **Reward:** Amber currently applies the bonus as a monthly credit offsetting the standard subscription fee.
