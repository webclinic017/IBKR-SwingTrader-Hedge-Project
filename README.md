# IBKR SwingTrader & Hedge Project
IBKR NinjaTrader Trade Automation Chart Module with Option Writer Linux Hedge Module.

This system includes both a NinjaTrader Chart Module Strategy that support both high frequency and swing trade modes. When a trade does not go as planned the Option Writer component takes over to write options against stock position that are going negative. It is designed to issues options within 2 months and watches those options to expiration. It will determine if the option should be closed via purchase, expiration or assignment. If the option is assigned the NinjaTrader module will take over management and the Option Writer will write an option against the positions if appropriate.


# Required System Components
This is a multi system deployment and it requires an individual Windows and Linux system. They can be Physical or Virtual Machines and it has been tested on VMware Workstation deployed on Linux or VMware ESXi. If a cloud environment is used it is important that the IP addresses be static.

## Windows 10 System
This system is primarily used for NinjaTrader 8 deployment
- Windows 10 (has not yet been tested with Windows 11)
- 8 Physical or Virtual CPUs
- 16 GB of Memory
- SSD 100GB Harddrive
- 1 Gb Internet Connection
- GPU are not yet enabled as part of the NinjaTrader Platform. Recommendation is to use this component headless.

## Linux System
This system is used for IBKR Trader Workstation, ibcAlpha IBC and Hedge_Project Option Writer Components
- Linux version 8
- 4 Physical or Virtula CPUs
- 8 GB of Memory
- SSD 40GB Harddrive
- 1 Gb Internet Connection


## IBKR Account Requirements
The IBKR account must be a Pro account with a minimum of 35K as this strategy module is configured for Day-Trading. The recommendation is to use the tiered pricing model as it allows for tighter ask/bid spreads. The minimum number of recommended positions is 25 across several sectors. It is configured for both pre and post market trading in the US Market (4am EST to 8pm EST) and therefore the Outside of RTH should be enabled. This account should have the ability to write covered and uncovered options.

## IBKR Market Data
Please have market data for all US Market Exchanges registered.

### Reference for IBKR
- IBKR Pro Account (https://www.interactivebrokers.com/en/index.php?f=45500)- 
- IBKR Outside of RTH (https://www.interactivebrokers.com/en/index.php?f=47551)
- IBKR Market Data (https://www.interactivebrokers.com/en/pricing/research-news-marketdata.php)
- IBKR Paper Trading Account (https://www.interactivebrokers.com/en/software/am/am/manageaccount/papertradingaccount.htm)


# NinjaTrader Strategy Module Requirement
This module is designed for Ninjatrader 8 and should be attached to 1-min chart as it is both a high fequency and swing trader modules that determines which module to use on a case by case basis. It should be in imported into NinjaTrader and then attached to the chart using the 'SL' strategy.

## NinjaTrader IBKR License
The license should be either leased or purchased as live trading is only available to a fully license product for connection to IBKR.

## NinjaTrader IBKR Connection 
Please see the Ninjatrader IBKR Connection guide and the recommendation is to use the TWS component for connection as oppose to the gateway. Unfortunately IBKR works best with the component as it provides the most stable account and market data API. For the High Fequency mode to function it needs a very stable and consist connection.

## Back Testings
This module uses other market indicators outside of individual position tick date to make buy/sell decisions and therefore NinjaTrading backtesting is not a viable option for testing this module. It is recommended to use IBKR Paper Trading account for testing. The NinjaTrader 8 simulation account is not viable as the Ask/Bid spread is not consistent with the market.

## Configuration
The module contains a majority of the necessary defaults; howerver, to support the embedded high frequency mode it is important the chart be set to 1-min bars. In order to run NinjaTrader headless you can use Windows Scheduler. 

### Reference for NinjaTrader
- NinjaTrader Hardware Requirements (https://ninjatrader.com/NinjaTrader-8-InstallationGuide)
- NinjaTrader Software (https://ninjatrader.com/BuyPlatform)
- NinJaTrader Connection Guide (https://ninjatrader.com/ConnectionGuides/Interactive-Brokers-Connection-Guide)


# IBKR Trader Workstation Component
In order to support NinjaTrader and Hedge_Product components you need to run the Trader Workstation component. It is recommended to running this component on the Linux system and to configure it to deployment headless using the IBC component. In order to work with the IBC component use must use the offline version.

## Configuration
In order to use the TWS API it must be enabled and configured to allow local connections. Use the reference below to use the complete the configuration. 

### Reference for Trader Workstation
- IBKR Trader Workstation version (https://www.interactivebrokers.com/en/index.php?f=16045)
- IBKR Trader Workstation installation (https://www.interactivebrokers.com/en/index.php?f=17863)
- IBKR Trader Workstation API Configuration (https://interactivebrokers.github.io/tws-api/initial_setup.html)
- IBC (https://github.com/IbcAlpha/IBC)


# IBKR Hedge Project Component
The Option Writer component should be run on the Linux systems that is shared with TWS/IBC components. It is designed to write options against long/short position of 100 or more shares. It works in conjunction with the NinjaTrader Strategy Module to properly size and managing positions that are currently hedged.

## Installation
The deployment script "hedgeInstallation.sh" should be executed with root permission and it will create directories and deploy the necessary components as well as the cron start and stop jobs in the EST timezone.

The environment configuration file is located in '/opt/local/env/Emv.conf' and prior to start the Account # should be updated. 

To start the system manually you can use 'systemctl start/stop hedge' service.
