.. _cmip6_compsets:

CMIP6 NorESM2 experiments setup
==============
A detailed overview of NorESM2 CMIP6 spin up and experiments including compsets, case names, restart files etc. can be found here  https://noresmhub.github.io/noresm-exp/intro.html


Please use the branch ``noresm2`` for reproducing CMIP6 results of NorESM2. For an overview, please see: https://noresm-docs.readthedocs.io/en/noresm2/access/releases_noresm20.html


In order to reproduce CMIP6 experiments:

Checkout the CMIP6 version of NorESM2
^^^^^
     How to obtain this version:  
::

     git clone https://github.com/NorESMhub/NorESM.git
     cd NorESM
     git tag --list (this should give you a list of the existing tags or releases)
     git checkout release-noresm2.0.5
     ./manage_externals/checkout_externals



List of all **coupled experiment** compsets can be found in::
     
     <noresm-base>/cime_config/config_compsets.xml

List of all **AMIP type** and **fixed SST** experiments can be found in 
::
     
     <noresm-base>/components/cam/cime_config/config_compsets.xml
     
**frc2 compsets**: Several CMIP6 experiments were run with frc2 compsets. The frc2 option uses differently organized emission files. The frc2 files are located in
::
  
  <PATH_TO_INPUTDATA>/noresm/inputdata/atm/cam/chem/emis/cmip6_emissions_version20190808
  
The frc2 files were created to avoid the occurrence of random mid-month model crashes on FRAM. These crashes are related to the reading of emission files. Compsets including **frc2** are using the frc2 emission files and include  

::

    NORESM2%FRC2
 
in the compset long name. 
 
 **Please note that running with frc2 compsets will not give bit-reproducible simulations compared to simulations run without frc2 in the compset name**



CMIP6 DECK Compsets
^^^^^

- **piControl**    : N1850 or N1850frc2
- **historical**   : NHIST or NHISTfrc2
- **abrupt-4xCO2** : NCO2x4cmip6 or NCO2x4cmip6frc2
- **1pctCO2**      : N1PCTcmip6 or N1PCTcmip6frc2

For a detailed overview of case names, experiment setup, restart files etc. please see: 

- NorESM2-MM: https://noresmhub.github.io/noresm-exp/content/noresm2_deck/noresm2_mm_deck.html
- NorESM2-LM: https://noresmhub.github.io/noresm-exp/content/noresm2_deck/noresm2_lm_deck.html

CMIP6 Scenario compsets (only frc2 compsets):
^^^^^

- **ssp126** : NSSP126frc2
- **ssp245** : NSSP245frc2
- **ssp370** : NSSP370frc2
- **ssp585** : NSSP585frc2

An overview of NorESM2 CMIP6 SSP experiments details, case names, restart files can be found here: https://noresmhub.github.io/noresm-exp/content/NSSPs/noresm2_mm_nssp.html 

CMIP6 AMIP and fixed SST compsets
^^^^^
- **amip** : NFHISTnorpddmsbc

Detailed information on NorESM2-LM AMIP simulation can be accessed here: https://noresmhub.github.io/noresm-exp/content/noresm2_deck/noresm2_lm_amip.html

Useful compsets for calculating aerosol ERF:

- Prescribed SST from NorESM piControl, 1850 aerosol emissions: NF1850norbc or NF1850frc2norbc 
- Prescribed SST from NorESM piControl, 2014 aerosol emissions: NF1850norbc_aer2014 or NF1850frc2norbc_aer2014


CMIP6 output
^^^^^

Please note that when building a case the ``user-mods-dir`` is important for the *output*, and should be changed according to your needs. Please see :ref:`experiments` sec. 2.1.5 and :ref:`standard_output` for further details.

The usermods under ``<noresm_base>/cime_config/usermods_dirs/`` include::

  cmip6_noresm_DECK (AEROFFL)    
  cmip6_noresm_hifreq (high frequency output, AEROFFL)    
  cmip6_noresm_hifreq_xaer (high frecuency output, AEROFFL and AEROCOM)   
  cmip6_noresm_keyCLIM (used for KeyCLIM experiments, AEROFFL)
  cmip6_noresm_xaer (AEROFFLand AEROCOM)    
  
To activate the cmip6_noresm_DECK usermod, run the create_newcase script with the option ``--user-mods-dir cmip6_noresm_DECK``. 

Remember that the amount of diagnostics and the output frequency have a huge impact on both the run time and storage. 

For more details, also see this folder ::

  <noresm_base>/cime_config/usermods_dirs


Reproduce CMIP6 piControl, historical and SSP5-8.5 experiments
======

piControl
^^^^^
``N1850`` is the alias for the NorESM compset for pre-industrial (1850) conditions. The long name for ``N1850`` is ::
  
  1850_CAM60%NORESM_CLM50%BGC-CROP_CICE%NORESM-CMIP6_BLOM%ECO_MOSART_SGLC_SWAV_BGC%BDRDDMS

and the long name for ``N1850frc2`` is ::

  1850_CAM60%NORESM%FRC2_CLM50%BGC-CROP_CICE%NORESM-CMIP6_BLOM%ECO_MOSART_SGLC_SWAV_BGC%BDRDDMS
  
- NorESM2-LM CMIP6 piControl was generated by the use of ``N1850``
- NorESM2-MM CMIP6 piControl was generated by the use of ``N1850frc2``

- **Create a piControl case**
     In ``<noresm-base>/cime/scripts/``
     
     - NorESM2-LM:

      ::

             ./create_newcase --case ../../cases/TEST_NorESM2-LM_piControl --compset N1850 --res f19_tn14 --machine fram --project $PROJECT --user-mods-dir cmip6_noresm_DECK   


     - NorESM2-MM:  

      ::

             ./create_newcase --case ../../cases/TEST_NorESM2-MM_piControl --compset N1850frc2 --res f09_tn14 --machine fram --project $PROJECT --user-mods-dir cmip6_noresm_DECK


- **Experiment settings**
   In your case folder (e.g. TEST_NorESM2-LM_piControl or TEST_NorESM2-MM_piControl)
   
   In ``env_run.xml``:
   
   - set ``RUN_TYPE`` to ``branch`` :
   
            ::
                
                 <group id="run_begin_stop_restart">
                   <entry id="RUN_TYPE" value="branch">

   - set ``RUN_REFCASE`` to the CMIP6 piControl casename and ``RUN_REFDATE`` to the start of the CMIP6 piControl experiment (or whatever year you need), i.e.
     
        - for  NorESM2-LM:
   
              ::
              
                   <entry id="RUN_REFCASE" value="N1850_f19_tn14_11062019">
         
                   <entry id="RUN_REFDATE" value="1600-01-01">
   
        - for NorESM2-MM:
     
              ::
            
                   <entry id="RUN_REFCASE" value="N1850_f09_tn14_20190913">
         
                   <entry id="RUN_REFDATE" value="1200-01-01">
                   
- **Restart files**
   Before submitting the job, please remeber to copy the restart and rpointer files to the run directory, e.g. for TEST_NorESM2-MM_piControl on BETZY ::
   
        cp /trd-project3/NS9560K/noresm/cases/N1850_f09_tn14_20190913/rest/1200-01-01-00000/* /cluster/projects/$PROJECT/$USER/noresm/TEST_NorESM2-MM_piControl/run/
        gunzip /cluster/projects/$PROJECT/$USER/noresm/TEST_NorESM2-MM_piControl/run/*.gz
   
Overview of piControl case names, detailed setup (machine settings, node settings etc.), raw output and restart files: 

- NorESM2-LM: https://noresmhub.github.io/noresm-exp/content/noresm2_deck/noresm2_lm_piC.html
- NorESM2-MM: https://noresmhub.github.io/noresm-exp/content/noresm2_deck/noresm2_mm_piC.html

Historical
^^^^^^

``NHIST`` is the alias for the NorESM compset for historical (1850-2014) conditions. The long name for ``NHIST`` is ::
   
      HIST_CAM60%NORESM_CLM50%BGC-CROP_CICE%NORESM-CMIP6_MICOM%ECO_MOSART_SGLC_SWAV_BGC%BDRDDMS

and for ``NHISTfrc2`` is ::
  
     HIST_CAM60%NORESM%FRC2_CLM50%BGC-CROP_CICE%NORESM-CMIP6_MICOM%ECO_MOSART_SGLC_SWAV_BGC%BDRDDMS
  
- NorESM2-LM CMIP6 historical experiments were generated by the use of ``NHIST``
- NorESM2-MM CMIP6 historical were generated by the use of ``NHISTfrc2``

- **Create a historical case**
     In ``<noresm-base>/cime/scripts/``
     
     - NorESM2-LM:

      ::

             ./create_newcase --case ../../cases/TEST_NorESM2-LM_historical --compset NHIST --res f19_tn14 --machine fram --project $PROJECT --user-mods-dir cmip6_noresm_xaer   


     - NorESM2-MM:  

      ::

             ./create_newcase --case ../../cases/TEST_NorESM2-MM_historical --compset NHISTfrc2 --res f09_tn14 --machine fram --project $PROJECT --user-mods-dir cmip6_noresm_DECK


- **Experiment settings**
   In your case folder (e.g. TEST_NorESM2-LM_historical or TEST_NorESM2-MM_historical)
   
   In ``env_run.xml``:
   
   - set ``RUN_TYPE`` to ``hybrid`` :
   
            ::
                
                 <group id="run_begin_stop_restart">
                   <entry id="RUN_TYPE" value="hybrid">

   - set ``RUN_REFCASE`` to the CMIP6 piControl casename (i.e. initial conditions) and ``RUN_REFDATE`` to the first year of the CMIP6 piControl experiment (or whatever year you need), i.e.
     
        - for  NorESM2-LM:
   
              ::
              
                   <entry id="RUN_REFCASE" value="N1850_f19_tn14_11062019">
         
                   <entry id="RUN_REFDATE" value="1600-01-01">
   
        - for NorESM2-MM:
     
              ::
            
                   <entry id="RUN_REFCASE" value="N1850_f09_tn14_20190913">
         
                   <entry id="RUN_REFDATE" value="1200-01-01">
 
- **Restart files**
   Before submitting the job, please remeber to copy the restart and rpointer files to the run directory, e.g. for TEST_NorESM2-LM_historical on BETZY ::
   
        cp /trd-project3/NS9560K/noresm/cases/N1850_f19_tn14_11062019/rest/1600-01-01-00000/* /cluster/projects/$PROJECT/$USER/noresm/TEST_NorESM2-LM_historical/run/
        gunzip /cluster/projects/$PROJECT/$USER/noresm/TEST_NorESM2-LM_historical/run/*.gz
        
        
Overview of historical case names, members, detailed setup (machine settings, node settings etc.), raw output and restart files: 

- NorESM2-LM: https://noresmhub.github.io/noresm-exp/content/noresm2_hist/noresm2_lm_hist.html
- NorESM2-MM: https://noresmhub.github.io/noresm-exp/content/noresm2_hist/noresm2_hist.html

SSP5-8.5
^^^^^
``NSSP585`` is the alias for the NorESM compset for projected (2015-2100) conditions. The scenario represents the high end of plausible future pathways. SSP5 is the only SSP with emissions high enough to produce the 8.5 W/m2 level of forcing in 2100. The long name for ``NSSP585`` is ::
   
    SSP585_CAM60%NORESM_CLM50%BGC-CROP_CICE%NORESM-CMIP6_MICOM%ECO_MOSART_SGLC_SWAV_BGC%BDRDDMS

and for ``NSSP585frc2`` is ::
  
    SSP585_CAM60%NORESM%FRC2_CLM50%BGC-CROP_CICE%NORESM-CMIP6_MICOM%ECO_MOSART_SGLC_SWAV_BGC%BDRDDMS
  
- Both NorESM2-LM and NorESM2-MM CMIP6 SSP5-8.5 experiments were generated by the use of ``NSSP585frc2``

- **Create a NSSP585 case**
     In ``<noresm-base>/cime/scripts/``
     
     - NorESM2-LM:

      ::

             ./create_newcase --case ../../cases/TEST_NorESM2-LM_ssp585 --compset NSSP585frc2 --res f19_tn14 --machine fram --project $PROJECT --user-mods-dir cmip6_noresm_hifreq_xaer  


     - NorESM2-MM:  

      ::

             ./create_newcase --case ../../cases/TEST_NorESM2-MM_ssp585 --compset NSSP585frc2 --res f09_tn14 --machine fram --project $PROJECT --user-mods-dir cmip6_noresm_hifreq_xaer


- **Experiment settings**
   In your case folder (e.g. TEST_NorESM2-LM_ssp585 or TEST_NorESM2-MM_ssp585)
   
   In ``env_run.xml``:
   
   - set ``RUN_TYPE`` to ``hybrid`` :
   
            ::
                
                 <group id="run_begin_stop_restart">
                   <entry id="RUN_TYPE" value="hybrid">

   - set ``RUN_REFCASE`` to the CMIP6 historical casename (i.e. initial conditions), please note that there are several historical members and the casename will depend on which member you need,  and ``RUN_REFDATE`` to the latest restart files of the CMIP6 historical experiment (or whatever year you need), i.e.
     
        - for  NorESM2-LM:
   
              ::
              
                   <entry id="RUN_REFCASE" value="NHIST_f19_tn14_20190710">
         
                   <entry id="RUN_REFDATE" value="2015-01-01">
                   
                   <entry id="RUN_STARTDATE" value="2015-01-01">
   
        - for NorESM2-MM:
     
              ::
            
                   <entry id="RUN_REFCASE" value="NHISTfrc2_f09_tn14_20191025"
         
                   <entry id="RUN_REFDATE" value="2015-01-01">
                   
                   <entry id="RUN_STARTDATE" value="2015-01-01">
   
 
- **Restart files**
   Before submitting the job, please remeber to copy the restart and rpointer files to the run directory, e.g. for TEST_NorESM2-LM_ssp585 on BETZY ::
   
        cp /trd-project3/NS9560K/noresm/cases/NHIST_f19_tn14_20190710/rest/2015-01-01-00000/* /cluster/projects/$PROJECT/$USER/noresm/TEST_NorESM2-LM_ssp585/run/
        gunzip /cluster/projects/$PROJECT/$USER/noresm/TEST_NorESM2-LM_ssp585/run/*.gz
        
Overview of scenario experiment case names, members, detailed setup (machine settings, node settings etc.), raw output and restart files: 

- NorESM2-LM and NorESM2-MM: https://noresmhub.github.io/noresm-exp/content/NSSPs/noresm2_mm_nssp.html

