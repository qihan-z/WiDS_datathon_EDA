wids\_dataXB\_eda
================

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ tibble  3.0.6     ✓ purrr   0.3.4
    ## ✓ tidyr   1.1.2     ✓ dplyr   1.0.4
    ## ✓ readr   1.4.0     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x lubridate::as.difftime() masks base::as.difftime()
    ## x lubridate::date()        masks base::date()
    ## x dplyr::filter()          masks stats::filter()
    ## x readr::guess_encoding()  masks rvest::guess_encoding()
    ## x lubridate::intersect()   masks base::intersect()
    ## x dplyr::lag()             masks stats::lag()
    ## x purrr::pluck()           masks rvest::pluck()
    ## x lubridate::setdiff()     masks base::setdiff()
    ## x lubridate::union()       masks base::union()

``` r
library(patchwork)
library(dplyr)
```

``` r
#a csv file on every variable's meaning
varDict <- read.csv("widsdatathon2021/DataDictionaryWiDS2021.csv") 

varDict
```

    ##               Category               Variable.Name        Unit.of.Measure
    ## 1           identifier                encounter_id                   None
    ## 2           identifier                 hospital_id                   None
    ## 3          demographic                         age                  Years
    ## 4          demographic                         bmi     kilograms/metres^2
    ## 5          demographic            elective_surgery                   None
    ## 6          demographic                   ethnicity                   None
    ## 7          demographic                      gender                   None
    ## 8          demographic                      height            centimetres
    ## 9          demographic       hospital_admit_source                   None
    ## 10         demographic            icu_admit_source                   None
    ## 11         demographic              icu_admit_type                   None
    ## 12         demographic                      icu_id                   None
    ## 13         demographic               icu_stay_type                   None
    ## 14         demographic                    icu_type                   None
    ## 15         demographic            pre_icu_los_days                   Days
    ## 16         demographic          readmission_status                   None
    ## 17         demographic                      weight              kilograms
    ## 18    APACHE covariate              albumin_apache                    g/L
    ## 19    APACHE covariate          apache_2_diagnosis                   None
    ## 20    APACHE covariate         apache_3j_diagnosis                   None
    ## 21    APACHE covariate       apache_post_operative                   None
    ## 22    APACHE covariate                  arf_apache                   None
    ## 23    APACHE covariate            bilirubin_apache             micromol/L
    ## 24    APACHE covariate                  bun_apache                 mmol/L
    ## 25    APACHE covariate           creatinine_apache             micromol/L
    ## 26    APACHE covariate                 fio2_apache               Fraction
    ## 27    APACHE covariate             gcs_eyes_apache                   None
    ## 28    APACHE covariate            gcs_motor_apache                   None
    ## 29    APACHE covariate           gcs_unable_apache                   None
    ## 30    APACHE covariate           gcs_verbal_apache                   None
    ## 31    APACHE covariate              glucose_apache                 mmol/L
    ## 32    APACHE covariate           heart_rate_apache       Beats per minute
    ## 33    APACHE covariate           hematocrit_apache               Fraction
    ## 34    APACHE covariate            intubated_apache                   None
    ## 35    APACHE covariate                  map_apache Millimetres of mercury
    ## 36    APACHE covariate                paco2_apache Millimetres of mercury
    ## 37    APACHE covariate         paco2_for_ph_apache Millimetres of mercury
    ## 38    APACHE covariate                 pao2_apache Millimetres of mercury
    ## 39    APACHE covariate                   ph_apache                   None
    ## 40    APACHE covariate             resprate_apache     Breaths per minute
    ## 41    APACHE covariate               sodium_apache                 mmol/L
    ## 42    APACHE covariate                 temp_apache        Degrees Celsius
    ## 43    APACHE covariate          urineoutput_apache            Millilitres
    ## 44    APACHE covariate           ventilated_apache                   None
    ## 45    APACHE covariate                  wbc_apache                 10^9/L
    ## 46              vitals      d1_diasbp_invasive_max Millimetres of mercury
    ## 47              vitals      d1_diasbp_invasive_min Millimetres of mercury
    ## 48              vitals               d1_diasbp_max Millimetres of mercury
    ## 49              vitals               d1_diasbp_min Millimetres of mercury
    ## 50              vitals   d1_diasbp_noninvasive_max Millimetres of mercury
    ## 51              vitals   d1_diasbp_noninvasive_min Millimetres of mercury
    ## 52              vitals            d1_heartrate_max       Beats per minute
    ## 53              vitals            d1_heartrate_min       Beats per minute
    ## 54              vitals         d1_mbp_invasive_max Millimetres of mercury
    ## 55              vitals         d1_mbp_invasive_min Millimetres of mercury
    ## 56              vitals                  d1_mbp_max Millimetres of mercury
    ## 57              vitals                  d1_mbp_min Millimetres of mercury
    ## 58              vitals      d1_mbp_noninvasive_max Millimetres of mercury
    ## 59              vitals      d1_mbp_noninvasive_min Millimetres of mercury
    ## 60              vitals             d1_resprate_max     Breaths per minute
    ## 61              vitals             d1_resprate_min     Breaths per minute
    ## 62              vitals                 d1_spo2_max             Percentage
    ## 63              vitals                 d1_spo2_min             Percentage
    ## 64              vitals       d1_sysbp_invasive_max Millimetres of mercury
    ## 65              vitals       d1_sysbp_invasive_min Millimetres of mercury
    ## 66              vitals                d1_sysbp_max Millimetres of mercury
    ## 67              vitals                d1_sysbp_min Millimetres of mercury
    ## 68              vitals    d1_sysbp_noninvasive_max Millimetres of mercury
    ## 69              vitals    d1_sysbp_noninvasive_min Millimetres of mercury
    ## 70              vitals                 d1_temp_max        Degrees Celsius
    ## 71              vitals                 d1_temp_min        Degrees Celsius
    ## 72              vitals      h1_diasbp_invasive_max Millimetres of mercury
    ## 73              vitals      h1_diasbp_invasive_min Millimetres of mercury
    ## 74              vitals               h1_diasbp_max Millimetres of mercury
    ## 75              vitals               h1_diasbp_min Millimetres of mercury
    ## 76              vitals   h1_diasbp_noninvasive_max Millimetres of mercury
    ## 77              vitals   h1_diasbp_noninvasive_min Millimetres of mercury
    ## 78              vitals            h1_heartrate_max       Beats per minute
    ## 79              vitals            h1_heartrate_min       Beats per minute
    ## 80              vitals         h1_mbp_invasive_max Millimetres of mercury
    ## 81              vitals         h1_mbp_invasive_min Millimetres of mercury
    ## 82              vitals                  h1_mbp_max Millimetres of mercury
    ## 83              vitals                  h1_mbp_min Millimetres of mercury
    ## 84              vitals      h1_mbp_noninvasive_max Millimetres of mercury
    ## 85              vitals      h1_mbp_noninvasive_min Millimetres of mercury
    ## 86              vitals             h1_resprate_max     Breaths per minute
    ## 87              vitals             h1_resprate_min     Breaths per minute
    ## 88              vitals                 h1_spo2_max             Percentage
    ## 89              vitals                 h1_spo2_min             Percentage
    ## 90              vitals       h1_sysbp_invasive_max Millimetres of mercury
    ## 91              vitals       h1_sysbp_invasive_min Millimetres of mercury
    ## 92              vitals                h1_sysbp_max Millimetres of mercury
    ## 93              vitals                h1_sysbp_min Millimetres of mercury
    ## 94              vitals    h1_sysbp_noninvasive_max Millimetres of mercury
    ## 95              vitals    h1_sysbp_noninvasive_min Millimetres of mercury
    ## 96              vitals                 h1_temp_max        Degrees Celsius
    ## 97              vitals                 h1_temp_min        Degrees Celsius
    ## 98                labs              d1_albumin_max                   None
    ## 99                labs              d1_albumin_min                    g/L
    ## 100               labs            d1_bilirubin_max             micromol/L
    ## 101               labs            d1_bilirubin_min             micromol/L
    ## 102               labs                  d1_bun_max                 mmol/L
    ## 103               labs                  d1_bun_min                 mmol/L
    ## 104               labs              d1_calcium_max                 mmol/L
    ## 105               labs              d1_calcium_min                 mmol/L
    ## 106               labs           d1_creatinine_max             micromol/L
    ## 107               labs           d1_creatinine_min             micromol/L
    ## 108               labs              d1_glucose_max                 mmol/L
    ## 109               labs              d1_glucose_min                 mmol/L
    ## 110               labs                 d1_hco3_max                 mmol/L
    ## 111               labs                 d1_hco3_min                   None
    ## 112               labs           d1_hemaglobin_max                   g/dL
    ## 113               labs           d1_hemaglobin_min                   g/dL
    ## 114               labs           d1_hematocrit_max               Fraction
    ## 115               labs           d1_hematocrit_min               Fraction
    ## 116               labs                  d1_inr_max             micromol/L
    ## 117               labs                  d1_inr_min             micromol/L
    ## 118               labs              d1_lactate_max                 mmol/L
    ## 119               labs              d1_lactate_min                 mmol/L
    ## 120               labs            d1_platelets_max                 10^9/L
    ## 121               labs            d1_platelets_min                 10^9/L
    ## 122               labs            d1_potassium_max                 mmol/L
    ## 123               labs            d1_potassium_min                 mmol/L
    ## 124               labs               d1_sodium_max                 mmol/L
    ## 125               labs               d1_sodium_min                 mmol/L
    ## 126               labs                  d1_wbc_max                 10^9/L
    ## 127               labs                  d1_wbc_min                 10^9/L
    ## 128               labs              h1_albumin_max                   None
    ## 129               labs              h1_albumin_min                    g/L
    ## 130               labs            h1_bilirubin_max             micromol/L
    ## 131               labs            h1_bilirubin_min             micromol/L
    ## 132               labs                  h1_bun_max                 mmol/L
    ## 133               labs                  h1_bun_min                 mmol/L
    ## 134               labs              h1_calcium_max                 mmol/L
    ## 135               labs              h1_calcium_min                 mmol/L
    ## 136               labs           h1_creatinine_max             micromol/L
    ## 137               labs           h1_creatinine_min             micromol/L
    ## 138               labs              h1_glucose_max                 mmol/L
    ## 139               labs              h1_glucose_min                 mmol/L
    ## 140               labs                 h1_hco3_max                 mmol/L
    ## 141               labs                 h1_hco3_min                   None
    ## 142               labs           h1_hemaglobin_max                   g/dL
    ## 143               labs           h1_hemaglobin_min                   g/dL
    ## 144               labs           h1_hematocrit_max               Fraction
    ## 145               labs           h1_hematocrit_min               Fraction
    ## 146               labs                  h1_inr_max             micromol/L
    ## 147               labs                  h1_inr_min             micromol/L
    ## 148               labs              h1_lactate_max                 mmol/L
    ## 149               labs              h1_lactate_min                 mmol/L
    ## 150               labs            h1_platelets_max                 10^9/L
    ## 151               labs            h1_platelets_min                 10^9/L
    ## 152               labs            h1_potassium_max                 mmol/L
    ## 153               labs            h1_potassium_min                 mmol/L
    ## 154               labs               h1_sodium_max                 mmol/L
    ## 155               labs               h1_sodium_min                 mmol/L
    ## 156               labs                  h1_wbc_max                 10^9/L
    ## 157               labs                  h1_wbc_min                 10^9/L
    ## 158     labs blood gas        d1_arterial_pco2_max Millimetres of mercury
    ## 159     labs blood gas        d1_arterial_pco2_min Millimetres of mercury
    ## 160     labs blood gas          d1_arterial_ph_max                   None
    ## 161     labs blood gas          d1_arterial_ph_min                   None
    ## 162     labs blood gas         d1_arterial_po2_max Millimetres of mercury
    ## 163     labs blood gas         d1_arterial_po2_min Millimetres of mercury
    ## 164     labs blood gas        d1_pao2fio2ratio_max               Fraction
    ## 165     labs blood gas        d1_pao2fio2ratio_min               Fraction
    ## 166     labs blood gas        h1_arterial_pco2_max Millimetres of mercury
    ## 167     labs blood gas        h1_arterial_pco2_min Millimetres of mercury
    ## 168     labs blood gas          h1_arterial_ph_max                   None
    ## 169     labs blood gas          h1_arterial_ph_min                   None
    ## 170     labs blood gas         h1_arterial_po2_max Millimetres of mercury
    ## 171     labs blood gas         h1_arterial_po2_min Millimetres of mercury
    ## 172     labs blood gas        h1_pao2fio2ratio_max               Fraction
    ## 173     labs blood gas        h1_pao2fio2ratio_min               Fraction
    ## 174 APACHE comorbidity                        aids                   None
    ## 175 APACHE comorbidity                   cirrhosis                   None
    ## 176 APACHE comorbidity             hepatic_failure                   None
    ## 177 APACHE comorbidity           immunosuppression                   None
    ## 178 APACHE comorbidity                    leukemia                   None
    ## 179 APACHE comorbidity                    lymphoma                   None
    ## 180 APACHE comorbidity solid_tumor_with_metastasis                   None
    ## 181    Target Variable           diabetes_mellitus                   None
    ##     Data.Type
    ## 1     integer
    ## 2     integer
    ## 3     numeric
    ## 4      string
    ## 5      binary
    ## 6      string
    ## 7      string
    ## 8     numeric
    ## 9      string
    ## 10     string
    ## 11     string
    ## 12    integer
    ## 13     string
    ## 14     string
    ## 15    numeric
    ## 16     binary
    ## 17    numeric
    ## 18    numeric
    ## 19     string
    ## 20     string
    ## 21     binary
    ## 22     binary
    ## 23    numeric
    ## 24    numeric
    ## 25    numeric
    ## 26    numeric
    ## 27    integer
    ## 28    integer
    ## 29     binary
    ## 30    integer
    ## 31    numeric
    ## 32    numeric
    ## 33    numeric
    ## 34     binary
    ## 35    numeric
    ## 36    numeric
    ## 37    numeric
    ## 38    numeric
    ## 39    numeric
    ## 40    numeric
    ## 41    numeric
    ## 42    numeric
    ## 43    numeric
    ## 44     binary
    ## 45    numeric
    ## 46    numeric
    ## 47    numeric
    ## 48    numeric
    ## 49    numeric
    ## 50    numeric
    ## 51    numeric
    ## 52    numeric
    ## 53    numeric
    ## 54    numeric
    ## 55    numeric
    ## 56    numeric
    ## 57    numeric
    ## 58    numeric
    ## 59    numeric
    ## 60    numeric
    ## 61    numeric
    ## 62    numeric
    ## 63    numeric
    ## 64    numeric
    ## 65    numeric
    ## 66    numeric
    ## 67    numeric
    ## 68    numeric
    ## 69    numeric
    ## 70    numeric
    ## 71    numeric
    ## 72    numeric
    ## 73    numeric
    ## 74    numeric
    ## 75    numeric
    ## 76    numeric
    ## 77    numeric
    ## 78    numeric
    ## 79    numeric
    ## 80    numeric
    ## 81    numeric
    ## 82    numeric
    ## 83    numeric
    ## 84    numeric
    ## 85    numeric
    ## 86    numeric
    ## 87    numeric
    ## 88    numeric
    ## 89    numeric
    ## 90    numeric
    ## 91    numeric
    ## 92    numeric
    ## 93    numeric
    ## 94    numeric
    ## 95    numeric
    ## 96    numeric
    ## 97    numeric
    ## 98    numeric
    ## 99    numeric
    ## 100   numeric
    ## 101   numeric
    ## 102   numeric
    ## 103   numeric
    ## 104   numeric
    ## 105   numeric
    ## 106   numeric
    ## 107   numeric
    ## 108   numeric
    ## 109   numeric
    ## 110   numeric
    ## 111   numeric
    ## 112   numeric
    ## 113   numeric
    ## 114   numeric
    ## 115   numeric
    ## 116   numeric
    ## 117   numeric
    ## 118   numeric
    ## 119   numeric
    ## 120   numeric
    ## 121   numeric
    ## 122   numeric
    ## 123   numeric
    ## 124   numeric
    ## 125   numeric
    ## 126   numeric
    ## 127   numeric
    ## 128   numeric
    ## 129   numeric
    ## 130   numeric
    ## 131   numeric
    ## 132   numeric
    ## 133   numeric
    ## 134   numeric
    ## 135   numeric
    ## 136   numeric
    ## 137   numeric
    ## 138   numeric
    ## 139   numeric
    ## 140   numeric
    ## 141   numeric
    ## 142   numeric
    ## 143   numeric
    ## 144   numeric
    ## 145   numeric
    ## 146   numeric
    ## 147   numeric
    ## 148   numeric
    ## 149   numeric
    ## 150   numeric
    ## 151   numeric
    ## 152   numeric
    ## 153   numeric
    ## 154   numeric
    ## 155   numeric
    ## 156   numeric
    ## 157   numeric
    ## 158   numeric
    ## 159   numeric
    ## 160   numeric
    ## 161   numeric
    ## 162   numeric
    ## 163   numeric
    ## 164   numeric
    ## 165   numeric
    ## 166   numeric
    ## 167   numeric
    ## 168   numeric
    ## 169   numeric
    ## 170   numeric
    ## 171   numeric
    ## 172   numeric
    ## 173   numeric
    ## 174    binary
    ## 175    binary
    ## 176    binary
    ## 177    binary
    ## 178    binary
    ## 179    binary
    ## 180    binary
    ## 181    binary
    ##                                                                                                                                                                                                                                                                                                                 Description
    ## 1                                                                                                                                                                                                                                                                     Unique identifier associated with a patient unit stay
    ## 2                                                                                                                                                                                                                                                                              Unique identifier associated with a hospital
    ## 3                                                                                                                                                                                                                                                                                  The age of the patient on unit admission
    ## 4                                                                                                                                                                                                                                                                       The body mass index of the person on unit admission
    ## 5                                                                                                                                                                                                                                       Whether the patient was admitted to the hospital for an elective surgical operation
    ## 6                                                                                                                                                                                                                                                     The common national or cultural tradition which the person belongs to
    ## 7                                                                                                                                                                                                                                                                                        The genotypical sex of the patient
    ## 8                                                                                                                                                                                                                                                                                The height of the person on unit admission
    ## 9                                                                                                                                                                                                                                                       The location of the patient prior to being admitted to the hospital
    ## 10                                                                                                                                                                                                                                                          The location of the patient prior to being admitted to the unit
    ## 11                                                                                                                                                                                                                                                                               The type of unit admission for the patient
    ## 12                                                                                                                                                                                                                                                       A unique identifier for the unit to which the patient was admitted
    ## 13                                                                                                                                                                                                                                                                                                                         
    ## 14                                                                                                                                                                                                                                       A classification which indicates the type of care the unit is capable of providing
    ## 15                                                                                                                                                                                                                                          The length of stay of the patient between hospital admission and unit admission
    ## 16                                                                                                                                                                                                                  Whether the current unit stay is the second (or greater) stay at an ICU within the same hospitalization
    ## 17                                                                                                                                                                                                                                                                   The weight (body mass) of the person on unit admission
    ## 18                                                                                                                                                                                                               The albumin concentration measured during the first 24 hours which results in the highest APACHE III score
    ## 19                                                                                                                                                                                                                                                                            The APACHE II diagnosis for the ICU admission
    ## 20                                                                                                                                                                                                                                The APACHE III-J sub-diagnosis code which best describes the reason for the ICU admission
    ## 21                                                                                                                                                                                                                                                   The APACHE operative status; 1 for post-operative, 0 for non-operative
    ## 22                                                                                                                                  Whether the patient had acute renal failure during the first 24 hours of their unit stay, defined as a 24 hour urine output <410ml, creatinine >=133 micromol/L and no chronic dialysis
    ## 23                                                                                                                                                                                                             The bilirubin concentration measured during the first 24 hours which results in the highest APACHE III score
    ## 24                                                                                                                                                                                                   The blood urea nitrogen concentration measured during the first 24 hours which results in the highest APACHE III score
    ## 25                                                                                                                                                                                                            The creatinine concentration measured during the first 24 hours which results in the highest APACHE III score
    ## 26                                                                                                                                                The fraction of inspired oxygen from the arterial blood gas taken during the first 24 hours of unit admission which produces the highest APACHE III score for oxygenation
    ## 27                                                                                                                                                                                     The eye opening component of the Glasgow Coma Scale measured during the first 24 hours which results in the highest APACHE III score
    ## 28                                                                                                                                                                                           The motor component of the Glasgow Coma Scale measured during the first 24 hours which results in the highest APACHE III score
    ## 29                                                                                                                                                                                                                                         Whether the Glasgow Coma Scale was unable to be assessed due to patient sedation
    ## 30                                                                                                                                                                                          The verbal component of the Glasgow Coma Scale measured during the first 24 hours which results in the highest APACHE III score
    ## 31                                                                                                                                                                                                               The glucose concentration measured during the first 24 hours which results in the highest APACHE III score
    ## 32                                                                                                                                                                                                                          The heart rate measured during the first 24 hours which results in the highest APACHE III score
    ## 33                                                                                                                                                                                                                          The hematocrit measured during the first 24 hours which results in the highest APACHE III score
    ## 34                                                                                                                                                                                                    Whether the patient was intubated at the time of the highest scoring arterial blood gas used in the oxygenation score
    ## 35                                                                                                                                                                                                              The mean arterial pressure measured during the first 24 hours which results in the highest APACHE III score
    ## 36                                                                                                                                         The partial pressure of carbon dioxide from the arterial blood gas taken during the first 24 hours of unit admission which produces the highest APACHE III score for oxygenation
    ## 37                                                                                                                               The partial pressure of carbon dioxide from the arterial blood gas taken during the first 24 hours of unit admission which produces the highest APACHE III score for acid-base disturbance
    ## 38                                                                                                                                                 The partial pressure of oxygen from the arterial blood gas taken during the first 24 hours of unit admission which produces the highest APACHE III score for oxygenation
    ## 39                                                                                                                                                               The pH from the arterial blood gas taken during the first 24 hours of unit admission which produces the highest APACHE III score for acid-base disturbance
    ## 40                                                                                                                                                                                                                    The respiratory rate measured during the first 24 hours which results in the highest APACHE III score
    ## 41                                                                                                                                                                                                                The sodium concentration measured during the first 24 hours which results in the highest APACHE III score
    ## 42                                                                                                                                                                                                                         The temperature measured during the first 24 hours which results in the highest APACHE III score
    ## 43                                                                                                                                                                                                                                                                            The total urine output for the first 24 hours
    ## 44                                           Whether the patient was invasively ventilated at the time of the highest scoring arterial blood gas using the oxygenation scoring algorithm, including any mode of positive pressure ventilation delivered through a circuit attached to an endo-tracheal tube or tracheostomy
    ## 45                                                                                                                                                                                                              The white blood cell count measured during the first 24 hours which results in the highest APACHE III score
    ## 46                                                                                                                                                                                                         The patient's highest diastolic blood pressure during the first 24 hours of their unit stay, invasively measured
    ## 47                                                                                                                                                                                                          The patient's lowest diastolic blood pressure during the first 24 hours of their unit stay, invasively measured
    ## 48                                                                                                                                                                                The patient's highest diastolic blood pressure during the first 24 hours of their unit stay, either non-invasively or invasively measured
    ## 49                                                                                                                                                                                 The patient's lowest diastolic blood pressure during the first 24 hours of their unit stay, either non-invasively or invasively measured
    ## 50                                                                                                                                                                                                     The patient's highest diastolic blood pressure during the first 24 hours of their unit stay, non-invasively measured
    ## 51                                                                                                                                                                                                      The patient's lowest diastolic blood pressure during the first 24 hours of their unit stay, non-invasively measured
    ## 52                                                                                                                                                                                                                                            The patient's highest heart rate during the first 24 hours of their unit stay
    ## 53                                                                                                                                                                                                                                             The patient's lowest heart rate during the first 24 hours of their unit stay
    ## 54                                                                                                                                                                                                              The patient's highest mean blood pressure during the first 24 hours of their unit stay, invasively measured
    ## 55                                                                                                                                                                                                               The patient's lowest mean blood pressure during the first 24 hours of their unit stay, invasively measured
    ## 56                                                                                                                                                                                     The patient's highest mean blood pressure during the first 24 hours of their unit stay, either non-invasively or invasively measured
    ## 57                                                                                                                                                                                      The patient's lowest mean blood pressure during the first 24 hours of their unit stay, either non-invasively or invasively measured
    ## 58                                                                                                                                                                                                          The patient's highest mean blood pressure during the first 24 hours of their unit stay, non-invasively measured
    ## 59                                                                                                                                                                                                           The patient's lowest mean blood pressure during the first 24 hours of their unit stay, non-invasively measured
    ## 60                                                                                                                                                                                                                                      The patient's highest respiratory rate during the first 24 hours of their unit stay
    ## 61                                                                                                                                                                                                                                       The patient's lowest respiratory rate during the first 24 hours of their unit stay
    ## 62                                                                                                                                                                                                                          The patient's highest peripheral oxygen saturation during the first 24 hours of their unit stay
    ## 63                                                                                                                                                                                                                           The patient's lowest peripheral oxygen saturation during the first 24 hours of their unit stay
    ## 64                                                                                                                                                                                                          The patient's highest systolic blood pressure during the first 24 hours of their unit stay, invasively measured
    ## 65                                                                                                                                                                                                           The patient's lowest systolic blood pressure during the first 24 hours of their unit stay, invasively measured
    ## 66                                                                                                                                                                                 The patient's highest systolic blood pressure during the first 24 hours of their unit stay, either non-invasively or invasively measured
    ## 67                                                                                                                                                                                  The patient's lowest systolic blood pressure during the first 24 hours of their unit stay, either non-invasively or invasively measured
    ## 68                                                                                                                                                                                                      The patient's highest systolic blood pressure during the first 24 hours of their unit stay, non-invasively measured
    ## 69                                                                                                                                                                                                       The patient's lowest systolic blood pressure during the first 24 hours of their unit stay, non-invasively measured
    ## 70                                                                                                                                                                                                                 The patient's highest core temperature during the first 24 hours of their unit stay, invasively measured
    ## 71                                                                                                                                                                                                                                       The patient's lowest core temperature during the first 24 hours of their unit stay
    ## 72                                                                                                                                                                                                             The patient's highest diastolic blood pressure during the first hour of their unit stay, invasively measured
    ## 73                                                                                                                                                                                                              The patient's lowest diastolic blood pressure during the first hour of their unit stay, invasively measured
    ## 74                                                                                                                                                                                    The patient's highest diastolic blood pressure during the first hour of their unit stay, either non-invasively or invasively measured
    ## 75                                                                                                                                                                                     The patient's lowest diastolic blood pressure during the first hour of their unit stay, either non-invasively or invasively measured
    ## 76                                                                                                                                                                                                         The patient's highest diastolic blood pressure during the first hour of their unit stay, non-invasively measured
    ## 77                                                                                                                                                                                                          The patient's lowest diastolic blood pressure during the first hour of their unit stay, non-invasively measured
    ## 78                                                                                                                                                                                                                                                The patient's highest heart rate during the first hour of their unit stay
    ## 79                                                                                                                                                                                                                                                 The patient's lowest heart rate during the first hour of their unit stay
    ## 80                                                                                                                                                                                                                  The patient's highest mean blood pressure during the first hour of their unit stay, invasively measured
    ## 81                                                                                                                                                                                                                   The patient's lowest mean blood pressure during the first hour of their unit stay, invasively measured
    ## 82                                                                                                                                                                                         The patient's highest mean blood pressure during the first hour of their unit stay, either non-invasively or invasively measured
    ## 83                                                                                                                                                                                          The patient's lowest mean blood pressure during the first hour of their unit stay, either non-invasively or invasively measured
    ## 84                                                                                                                                                                                                              The patient's highest mean blood pressure during the first hour of their unit stay, non-invasively measured
    ## 85                                                                                                                                                                                                               The patient's lowest mean blood pressure during the first hour of their unit stay, non-invasively measured
    ## 86                                                                                                                                                                                                                                          The patient's highest respiratory rate during the first hour of their unit stay
    ## 87                                                                                                                                                                                                                                           The patient's lowest respiratory rate during the first hour of their unit stay
    ## 88                                                                                                                                                                                                                              The patient's highest peripheral oxygen saturation during the first hour of their unit stay
    ## 89                                                                                                                                                                                                                               The patient's lowest peripheral oxygen saturation during the first hour of their unit stay
    ## 90                                                                                                                                                                                                              The patient's highest systolic blood pressure during the first hour of their unit stay, invasively measured
    ## 91                                                                                                                                                                                                               The patient's lowest systolic blood pressure during the first hour of their unit stay, invasively measured
    ## 92                                                                                                                                                                                     The patient's highest systolic blood pressure during the first hour of their unit stay, either non-invasively or invasively measured
    ## 93                                                                                                                                                                                      The patient's lowest systolic blood pressure during the first hour of their unit stay, either non-invasively or invasively measured
    ## 94                                                                                                                                                                                                          The patient's highest systolic blood pressure during the first hour of their unit stay, non-invasively measured
    ## 95                                                                                                                                                                                                           The patient's lowest systolic blood pressure during the first hour of their unit stay, non-invasively measured
    ## 96                                                                                                                                                                                                                     The patient's highest core temperature during the first hour of their unit stay, invasively measured
    ## 97                                                                                                                                                                                                                                           The patient's lowest core temperature during the first hour of their unit stay
    ## 98                                                                                                                                                                                                              The lowest albumin concentration of the patient in their serum during the first 24 hours of their unit stay
    ## 99                                                                                                                                                                                                              The lowest albumin concentration of the patient in their serum during the first 24 hours of their unit stay
    ## 100                                                                                                                                                                                                The highest bilirubin concentration of the patient in their serum or plasma during the first 24 hours of their unit stay
    ## 101                                                                                                                                                                                                 The lowest bilirubin concentration of the patient in their serum or plasma during the first 24 hours of their unit stay
    ## 102                                                                                                                                                                                      The highest blood urea nitrogen concentration of the patient in their serum or plasma during the first 24 hours of their unit stay
    ## 103                                                                                                                                                                                       The lowest blood urea nitrogen concentration of the patient in their serum or plasma during the first 24 hours of their unit stay
    ## 104                                                                                                                                                                                                            The highest calcium concentration of the patient in their serum during the first 24 hours of their unit stay
    ## 105                                                                                                                                                                                                             The lowest calcium concentration of the patient in their serum during the first 24 hours of their unit stay
    ## 106                                                                                                                                                                                               The highest creatinine concentration of the patient in their serum or plasma during the first 24 hours of their unit stay
    ## 107                                                                                                                                                                                                The lowest creatinine concentration of the patient in their serum or plasma during the first 24 hours of their unit stay
    ## 108                                                                                                                                                                                                  The highest glucose concentration of the patient in their serum or plasma during the first 24 hours of their unit stay
    ## 109                                                                                                                                                                                                   The lowest glucose concentration of the patient in their serum or plasma during the first 24 hours of their unit stay
    ## 110                                                                                                                                                                                             The highest bicarbonate concentration for the patient in their serum or plasma during the first 24 hours of their unit stay
    ## 111                                                                                                                                                                                              The lowest bicarbonate concentration for the patient in their serum or plasma during the first 24 hours of their unit stay
    ## 112                                                                                                                                                                                                                       The highest hemoglobin concentration for the patient during the first 24 hours of their unit stay
    ## 113                                                                                                                                                                                                                        The lowest hemoglobin concentration for the patient during the first 24 hours of their unit stay
    ## 114                                                                                                                                                                             The highest volume proportion of red blood cells in a patient's blood during the first 24 hours of their unit stay, expressed as a fraction
    ## 115                                                                                                                                                                              The lowest volume proportion of red blood cells in a patient's blood during the first 24 hours of their unit stay, expressed as a fraction
    ## 116                                                                                                                                                                                                                 The highest international normalized ratio for the patient during the first 24 hours of their unit stay
    ## 117                                                                                                                                                                                                                  The lowest international normalized ratio for the patient during the first 24 hours of their unit stay
    ## 118                                                                                                                                                                                                 The highest lactate concentration for the patient in their serum or plasma during the first 24 hours of their unit stay
    ## 119                                                                                                                                                                                                  The lowest lactate concentration for the patient in their serum or plasma during the first 24 hours of their unit stay
    ## 120                                                                                                                                                                                                                                 The highest platelet count for the patient during the first 24 hours of their unit stay
    ## 121                                                                                                                                                                                                                                  The lowest platelet count for the patient during the first 24 hours of their unit stay
    ## 122                                                                                                                                                                                               The highest potassium concentration for the patient in their serum or plasma during the first 24 hours of their unit stay
    ## 123                                                                                                                                                                                                The lowest potassium concentration for the patient in their serum or plasma during the first 24 hours of their unit stay
    ## 124                                                                                                                                                                                                  The highest sodium concentration for the patient in their serum or plasma during the first 24 hours of their unit stay
    ## 125                                                                                                                                                                                                   The lowest sodium concentration for the patient in their serum or plasma during the first 24 hours of their unit stay
    ## 126                                                                                                                                                                                                                         The highest white blood cell count for the patient during the first 24 hours of their unit stay
    ## 127                                                                                                                                                                                                                          The lowest white blood cell count for the patient during the first 24 hours of their unit stay
    ## 128                                                                                                                                                                                                                 The lowest albumin concentration of the patient in their serum during the first hour of their unit stay
    ## 129                                                                                                                                                                                                                 The lowest albumin concentration of the patient in their serum during the first hour of their unit stay
    ## 130                                                                                                                                                                                                    The highest bilirubin concentration of the patient in their serum or plasma during the first hour of their unit stay
    ## 131                                                                                                                                                                                                     The lowest bilirubin concentration of the patient in their serum or plasma during the first hour of their unit stay
    ## 132                                                                                                                                                                                          The highest blood urea nitrogen concentration of the patient in their serum or plasma during the first hour of their unit stay
    ## 133                                                                                                                                                                                           The lowest blood urea nitrogen concentration of the patient in their serum or plasma during the first hour of their unit stay
    ## 134                                                                                                                                                                                                                The highest calcium concentration of the patient in their serum during the first hour of their unit stay
    ## 135                                                                                                                                                                                                                 The lowest calcium concentration of the patient in their serum during the first hour of their unit stay
    ## 136                                                                                                                                                                                                   The highest creatinine concentration of the patient in their serum or plasma during the first hour of their unit stay
    ## 137                                                                                                                                                                                                    The lowest creatinine concentration of the patient in their serum or plasma during the first hour of their unit stay
    ## 138                                                                                                                                                                                                      The highest glucose concentration of the patient in their serum or plasma during the first hour of their unit stay
    ## 139                                                                                                                                                                                                       The lowest glucose concentration of the patient in their serum or plasma during the first hour of their unit stay
    ## 140                                                                                                                                                                                                 The highest bicarbonate concentration for the patient in their serum or plasma during the first hour of their unit stay
    ## 141                                                                                                                                                                                                  The lowest bicarbonate concentration for the patient in their serum or plasma during the first hour of their unit stay
    ## 142                                                                                                                                                                                                                           The highest hemoglobin concentration for the patient during the first hour of their unit stay
    ## 143                                                                                                                                                                                                                            The lowest hemoglobin concentration for the patient during the first hour of their unit stay
    ## 144                                                                                                                                                                                 The highest volume proportion of red blood cells in a patient's blood during the first hour of their unit stay, expressed as a fraction
    ## 145                                                                                                                                                                                  The lowest volume proportion of red blood cells in a patient's blood during the first hour of their unit stay, expressed as a fraction
    ## 146                                                                                                                                                                                                                     The highest international normalized ratio for the patient during the first hour of their unit stay
    ## 147                                                                                                                                                                                                                      The lowest international normalized ratio for the patient during the first hour of their unit stay
    ## 148                                                                                                                                                                                                     The highest lactate concentration for the patient in their serum or plasma during the first hour of their unit stay
    ## 149                                                                                                                                                                                                      The lowest lactate concentration for the patient in their serum or plasma during the first hour of their unit stay
    ## 150                                                                                                                                                                                                                                     The highest platelet count for the patient during the first hour of their unit stay
    ## 151                                                                                                                                                                                                                                      The lowest platelet count for the patient during the first hour of their unit stay
    ## 152                                                                                                                                                                                                   The highest potassium concentration for the patient in their serum or plasma during the first hour of their unit stay
    ## 153                                                                                                                                                                                                    The lowest potassium concentration for the patient in their serum or plasma during the first hour of their unit stay
    ## 154                                                                                                                                                                                                      The highest sodium concentration for the patient in their serum or plasma during the first hour of their unit stay
    ## 155                                                                                                                                                                                                       The lowest sodium concentration for the patient in their serum or plasma during the first hour of their unit stay
    ## 156                                                                                                                                                                                                                             The highest white blood cell count for the patient during the first hour of their unit stay
    ## 157                                                                                                                                                                                                                              The lowest white blood cell count for the patient during the first hour of their unit stay
    ## 158                                                                                                                                                                                                    The highest arterial partial pressure of carbon dioxide for the patient during the first 24 hours of their unit stay
    ## 159                                                                                                                                                                                                     The lowest arterial partial pressure of carbon dioxide for the patient during the first 24 hours of their unit stay
    ## 160                                                                                                                                                                                                                                    The highest arterial pH for the patient during the first 24 hours of their unit stay
    ## 161                                                                                                                                                                                                                                     The lowest arterial pH for the patient during the first 24 hours of their unit stay
    ## 162                                                                                                                                                                                                            The highest arterial partial pressure of oxygen for the patient during the first 24 hours of their unit stay
    ## 163                                                                                                                                                                                                             The lowest arterial partial pressure of oxygen for the patient during the first 24 hours of their unit stay
    ## 164                                                                                                                                                                                                                    The highest fraction of inspired oxygen for the patient during the first 24 hours of their unit stay
    ## 165                                                                                                                                                                                                                     The lowest fraction of inspired oxygen for the patient during the first 24 hours of their unit stay
    ## 166                                                                                                                                                                                                        The highest arterial partial pressure of carbon dioxide for the patient during the first hour of their unit stay
    ## 167                                                                                                                                                                                                         The lowest arterial partial pressure of carbon dioxide for the patient during the first hour of their unit stay
    ## 168                                                                                                                                                                                                                                        The highest arterial pH for the patient during the first hour of their unit stay
    ## 169                                                                                                                                                                                                                                         The lowest arterial pH for the patient during the first hour of their unit stay
    ## 170                                                                                                                                                                                                                The highest arterial partial pressure of oxygen for the patient during the first hour of their unit stay
    ## 171                                                                                                                                                                                                                 The lowest arterial partial pressure of oxygen for the patient during the first hour of their unit stay
    ## 172                                                                                                                                                                                                                        The highest fraction of inspired oxygen for the patient during the first hour of their unit stay
    ## 173                                                                                                                                                                                                                         The lowest fraction of inspired oxygen for the patient during the first hour of their unit stay
    ## 174                                                                                                                                                                                                   Whether the patient has a definitive diagnosis of acquired immune deficiency syndrome (AIDS) (not HIV positive alone)
    ## 175                                   Whether the patient has a history of heavy alcohol use with portal hypertension and varices, other causes of cirrhosis with evidence of portal hypertension and varices, or biopsy proven cirrhosis. This comorbidity does not apply to patients with a functioning liver transplant.
    ## 176                                                                                                                                                                      Whether the patient has cirrhosis and additional complications including jaundice and ascites, upper GI bleeding, hepatic encephalopathy, or coma.
    ## 177 Whether the patient has their immune system suppressed within six months prior to ICU admission for any of the following reasons; radiation therapy, chemotherapy, use of non-cytotoxic immunosuppressive drugs, high dose steroids (at least 0.3 mg/kg/day of methylprednisolone or equivalent for at least 6 months).
    ## 178                                                                                                                                                                          Whether the patient has been diagnosed with acute or chronic myelogenous leukemia, acute or chronic lymphocytic leukemia, or multiple myeloma.
    ## 179                                                                                                                                                                                                                                                       Whether the patient has been diagnosed with non-Hodgkin lymphoma.
    ## 180                                                                                                                                                                                  Whether the patient has been diagnosed with any solid tumor carcinoma (including malignant melanoma) which has evidence of metastasis.
    ## 181                                                                                                                                                                                                                                       Whether the patient has been diagnosed with diabetes mellitus, a chronic disease.
    ##              Example
    ## 1               None
    ## 2               None
    ## 3               None
    ## 4               21.5
    ## 5                  0
    ## 6          Caucasian
    ## 7                  F
    ## 8                180
    ## 9               Home
    ## 10    Operating room
    ## 11    Cardiothoracic
    ## 12              None
    ## 13              None
    ## 14  Neurological ICU
    ## 15               3.5
    ## 16                 0
    ## 17                80
    ## 18                30
    ## 19               308
    ## 20              1405
    ## 21                 1
    ## 22                 0
    ## 23                20
    ## 24              None
    ## 25                70
    ## 26              0.21
    ## 27                 4
    ## 28                 6
    ## 29                 1
    ## 30                 5
    ## 31                 5
    ## 32                75
    ## 33               0.4
    ## 34                 0
    ## 35              None
    ## 36                40
    ## 37                40
    ## 38                80
    ## 39               7.4
    ## 40                14
    ## 41               145
    ## 42                33
    ## 43              2000
    ## 44                 1
    ## 45                10
    ## 46                60
    ## 47                60
    ## 48                60
    ## 49                60
    ## 50                60
    ## 51                60
    ## 52                75
    ## 53                75
    ## 54                80
    ## 55                80
    ## 56                80
    ## 57                80
    ## 58                80
    ## 59                80
    ## 60                14
    ## 61                14
    ## 62              None
    ## 63               100
    ## 64               120
    ## 65               120
    ## 66               120
    ## 67               120
    ## 68               120
    ## 69               120
    ## 70                33
    ## 71                33
    ## 72                60
    ## 73                60
    ## 74                60
    ## 75                60
    ## 76                60
    ## 77                60
    ## 78                75
    ## 79                75
    ## 80                80
    ## 81                80
    ## 82                80
    ## 83                80
    ## 84                80
    ## 85                80
    ## 86                14
    ## 87                14
    ## 88              None
    ## 89               100
    ## 90               120
    ## 91               120
    ## 92               120
    ## 93               120
    ## 94               120
    ## 95               120
    ## 96                33
    ## 97                33
    ## 98                30
    ## 99                30
    ## 100               20
    ## 101               20
    ## 102                5
    ## 103                5
    ## 104              2.5
    ## 105              2.5
    ## 106               70
    ## 107               70
    ## 108                5
    ## 109                5
    ## 110               30
    ## 111               30
    ## 112               10
    ## 113               10
    ## 114              0.4
    ## 115              0.4
    ## 116                1
    ## 117                1
    ## 118                1
    ## 119                1
    ## 120              200
    ## 121              200
    ## 122              3.8
    ## 123              3.8
    ## 124              145
    ## 125              145
    ## 126               10
    ## 127               10
    ## 128               30
    ## 129               30
    ## 130               20
    ## 131               20
    ## 132                5
    ## 133                5
    ## 134              2.5
    ## 135              2.5
    ## 136               70
    ## 137               70
    ## 138                5
    ## 139                5
    ## 140               30
    ## 141               30
    ## 142               10
    ## 143               10
    ## 144              0.4
    ## 145              0.4
    ## 146                1
    ## 147                1
    ## 148                1
    ## 149                1
    ## 150              200
    ## 151              200
    ## 152              3.8
    ## 153              3.8
    ## 154              145
    ## 155              145
    ## 156               10
    ## 157               10
    ## 158               40
    ## 159               40
    ## 160              7.4
    ## 161              7.4
    ## 162               80
    ## 163               80
    ## 164             0.21
    ## 165             0.21
    ## 166               40
    ## 167               40
    ## 168              7.4
    ## 169              7.4
    ## 170               80
    ## 171               80
    ## 172             0.21
    ## 173             0.21
    ## 174                1
    ## 175                1
    ## 176                1
    ## 177                1
    ## 178                1
    ## 179                1
    ## 180                1
    ## 181                1

``` r
#EDA this dataset
Diabetes <- read.csv("widsdatathon2021/TrainingWiDS2021.csv") 
```

\#\#\#EDA Question & TO DOs What are the missing values and their
percentage in each column? -How values should we use to fill in the NAs?
What are variables related to diabetes? -Every BMI-related variables?
-Ethnicity?Age? -Do we use every chemical variables (albumin, calcium…)
in our model? What columns can we drop? -every ID

``` r
glimpse(Diabetes)
```

    ## Rows: 130,157
    ## Columns: 181
    ## $ X                           <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13…
    ## $ encounter_id                <int> 214826, 246060, 276985, 262220, 201746, 1…
    ## $ hospital_id                 <int> 118, 81, 118, 118, 33, 83, 83, 33, 118, 1…
    ## $ age                         <int> 68, 77, 25, 81, 19, 67, 59, 70, 45, 50, 7…
    ## $ bmi                         <dbl> 22.73280, 27.42188, 31.95275, 22.63555, N…
    ## $ elective_surgery            <int> 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1,…
    ## $ ethnicity                   <chr> "Caucasian", "Caucasian", "Caucasian", "C…
    ## $ gender                      <chr> "M", "F", "F", "F", "M", "M", "F", "M", "…
    ## $ height                      <dbl> 180.3, 160.0, 172.7, 165.1, 188.0, 190.5,…
    ## $ hospital_admit_source       <chr> "Floor", "Floor", "Emergency Department",…
    ## $ icu_admit_source            <chr> "Floor", "Floor", "Accident & Emergency",…
    ## $ icu_id                      <int> 92, 90, 93, 92, 91, 95, 95, 91, 114, 114,…
    ## $ icu_stay_type               <chr> "admit", "admit", "admit", "admit", "admi…
    ## $ icu_type                    <chr> "CTICU", "Med-Surg ICU", "Med-Surg ICU", …
    ## $ pre_icu_los_days            <dbl> 0.541666667, 0.927777778, 0.000694444, 0.…
    ## $ readmission_status          <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ weight                      <dbl> 73.9, 70.2, 95.3, 61.7, NA, 100.0, 156.6,…
    ## $ albumin_apache              <dbl> 2.3, NA, NA, NA, NA, NA, NA, NA, 2.7, 3.6…
    ## $ apache_2_diagnosis          <int> 113, 108, 122, 203, 119, 301, 108, 113, 1…
    ## $ apache_3j_diagnosis         <dbl> 502.01, 203.01, 703.03, 1206.03, 601.01, …
    ## $ apache_post_operative       <int> 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1,…
    ## $ arf_apache                  <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ bilirubin_apache            <dbl> 0.4, NA, NA, NA, NA, NA, NA, NA, 0.2, 0.4…
    ## $ bun_apache                  <dbl> 31, 9, NA, NA, NA, 13, 18, 48, 15, 10, NA…
    ## $ creatinine_apache           <dbl> 2.51, 0.56, NA, NA, NA, 0.71, 0.78, 2.05,…
    ## $ fio2_apache                 <dbl> NA, 1.0, NA, 0.6, NA, NA, 1.0, NA, 1.0, N…
    ## $ gcs_eyes_apache             <int> 3, 1, 3, 4, NA, 4, 4, 4, 4, 4, 4, 4, 4, 4…
    ## $ gcs_motor_apache            <int> 6, 3, 6, 6, NA, 6, 6, 6, 6, 6, 6, 6, 6, 6…
    ## $ gcs_unable_apache           <int> 0, 0, 0, 0, NA, 0, 0, 0, 0, 0, 0, 0, 0, 0…
    ## $ gcs_verbal_apache           <int> 4, 1, 5, 5, NA, 5, 5, 5, 5, 5, 5, 4, 5, 5…
    ## $ glucose_apache              <dbl> 168, 145, NA, 185, NA, 156, 197, 164, 380…
    ## $ heart_rate_apache           <int> 118, 120, 102, 114, 60, 113, 133, 120, 82…
    ## $ hematocrit_apache           <dbl> 27.4, 36.9, NA, 25.9, NA, 44.2, 33.5, 22.…
    ## $ intubated_apache            <int> 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0,…
    ## $ map_apache                  <dbl> 40, 46, 68, 60, 103, 130, 138, 60, 66, 58…
    ## $ paco2_apache                <dbl> NA, 37, NA, 30, NA, NA, 43, NA, 60, NA, N…
    ## $ paco2_for_ph_apache         <dbl> NA, 37, NA, 30, NA, NA, 43, NA, 60, NA, N…
    ## $ pao2_apache                 <dbl> NA, 51, NA, 142, NA, NA, 370, NA, 92, NA,…
    ## $ ph_apache                   <dbl> NA, 7.45, NA, 7.39, NA, NA, 7.42, NA, 7.1…
    ## $ resprate_apache             <dbl> 36, 33, 37, 4, 16, 35, 53, 28, 14, 46, 15…
    ## $ sodium_apache               <dbl> 134, 145, NA, NA, NA, 137, 135, 140, 142,…
    ## $ temp_apache                 <dbl> 39.3, 35.1, 36.7, 34.8, 36.7, 36.6, 35.0,…
    ## $ urineoutput_apache          <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    ## $ ventilated_apache           <int> 0, 1, 0, 1, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0,…
    ## $ wbc_apache                  <dbl> 14.1, 12.7, NA, 8.0, NA, 10.9, 5.9, 12.8,…
    ## $ d1_diasbp_invasive_max      <int> 46, NA, NA, 62, NA, NA, 107, NA, 64, 74, …
    ## $ d1_diasbp_invasive_min      <int> 32, NA, NA, 30, NA, NA, 65, NA, 52, 57, 5…
    ## $ d1_diasbp_max               <int> 68, 95, 88, 48, 99, 100, 76, 84, 65, 83, …
    ## $ d1_diasbp_min               <int> 37, 31, 48, 42, 57, 61, 68, 46, 59, 48, 5…
    ## $ d1_diasbp_noninvasive_max   <int> 68, 95, 88, 48, 99, 100, 76, 84, 65, 83, …
    ## $ d1_diasbp_noninvasive_min   <int> 37, 31, 48, 42, 57, 61, 68, 46, 59, 48, 5…
    ## $ d1_heartrate_max            <int> 119, 118, 96, 116, 89, 113, 112, 118, 82,…
    ## $ d1_heartrate_min            <int> 72, 72, 68, 92, 60, 83, 70, 86, 82, 57, 6…
    ## $ d1_mbp_invasive_max         <int> 66, NA, NA, 92, NA, NA, 138, NA, 72, 92, …
    ## $ d1_mbp_invasive_min         <int> 40, NA, NA, 52, NA, NA, 84, NA, 66, 73, 6…
    ## $ d1_mbp_max                  <dbl> 89, 120, 102, 84, 104, 127, 117, 114, 93,…
    ## $ d1_mbp_min                  <dbl> 46, 38, 68, 84, 90, 80, 97, 60, 71, 59, 7…
    ## $ d1_mbp_noninvasive_max      <dbl> 89, 120, 102, 84, 104, 127, 117, 114, 93,…
    ## $ d1_mbp_noninvasive_min      <dbl> 46, 38, 68, 84, 90, 80, 97, 60, 71, 59, 7…
    ## $ d1_resprate_max             <int> 34, 32, 21, 23, 18, 32, 38, 28, 24, 44, 2…
    ## $ d1_resprate_min             <int> 10, 12, 8, 7, 16, 10, 16, 12, 19, 14, 14,…
    ## $ d1_spo2_max                 <int> 100, 100, 98, 100, 100, 97, 100, 100, 97,…
    ## $ d1_spo2_min                 <int> 74, 70, 91, 95, 96, 91, 87, 92, 97, 96, 9…
    ## $ d1_sysbp_invasive_max       <int> 122, NA, NA, 164, NA, NA, 191, NA, 94, 12…
    ## $ d1_sysbp_invasive_min       <int> 64, NA, NA, 78, NA, NA, 116, NA, 72, 103,…
    ## $ d1_sysbp_max                <int> 131, 159, 148, 158, 147, 173, 151, 147, 1…
    ## $ d1_sysbp_min                <int> 73, 67, 105, 84, 120, 107, 133, 71, 98, 7…
    ## $ d1_sysbp_noninvasive_max    <int> 131, 159, 148, 158, 147, 173, 151, 147, 1…
    ## $ d1_sysbp_noninvasive_min    <dbl> 73, 67, 105, 84, 120, 107, 133, 71, 98, 7…
    ## $ d1_temp_max                 <dbl> 39.9, 36.3, 37.0, 38.0, 37.2, 36.8, 37.2,…
    ## $ d1_temp_min                 <dbl> 37.2, 35.1, 36.7, 34.8, 36.7, 36.6, 35.0,…
    ## $ h1_diasbp_invasive_max      <int> NA, NA, NA, 62, NA, NA, 107, NA, 64, 73, …
    ## $ h1_diasbp_invasive_min      <int> NA, NA, NA, 44, NA, NA, 79, NA, 52, 62, 6…
    ## $ h1_diasbp_max               <int> 68, 61, 88, 62, 99, 89, 107, 74, 65, 83, …
    ## $ h1_diasbp_min               <int> 63, 48, 58, 44, 68, 89, 79, 55, 59, 61, 5…
    ## $ h1_diasbp_noninvasive_max   <int> 68, 61, 88, NA, 99, 89, NA, 74, 65, 83, 7…
    ## $ h1_diasbp_noninvasive_min   <int> 63, 48, 58, NA, 68, 89, NA, 55, 59, 61, 5…
    ## $ h1_heartrate_max            <int> 119, 114, 96, 100, 89, 83, 79, 118, 82, 9…
    ## $ h1_heartrate_min            <int> 108, 100, 78, 96, 76, 83, 72, 114, 82, 60…
    ## $ h1_mbp_invasive_max         <dbl> NA, NA, NA, 92, NA, NA, 138, NA, 72, 92, …
    ## $ h1_mbp_invasive_min         <int> NA, NA, NA, 71, NA, NA, 115, NA, 66, 78, …
    ## $ h1_mbp_max                  <dbl> 86, 85, 91, 92, 104, 111, 117, 88, 93, 10…
    ## $ h1_mbp_min                  <dbl> 85, 57, 83, 71, 92, 111, 117, 60, 71, 77,…
    ## $ h1_mbp_noninvasive_max      <dbl> 86, 85, 91, NA, 104, 111, 117, 88, 93, 10…
    ## $ h1_mbp_noninvasive_min      <dbl> 85, 57, 83, NA, 92, 111, 117, 60, 71, 77,…
    ## $ h1_resprate_max             <int> 26, 31, 20, 12, NA, 12, 18, 28, 24, 29, 2…
    ## $ h1_resprate_min             <int> 18, 28, 16, 11, NA, 12, 18, 26, 19, 17, 1…
    ## $ h1_spo2_max                 <int> 100, 95, 98, 100, 100, 97, 100, 96, 97, 1…
    ## $ h1_spo2_min                 <int> 74, 70, 91, 99, 100, 97, 100, 92, 97, 96,…
    ## $ h1_sysbp_invasive_max       <int> NA, NA, NA, 136, NA, NA, 191, NA, 94, 126…
    ## $ h1_sysbp_invasive_min       <dbl> NA, NA, NA, 106, NA, NA, 163, NA, 72, 106…
    ## $ h1_sysbp_max                <int> 131, 95, 148, 136, 130, 143, 191, 119, 10…
    ## $ h1_sysbp_min                <int> 115, 71, 124, 106, 120, 143, 163, 106, 98…
    ## $ h1_sysbp_noninvasive_max    <int> 131, 95, 148, NA, 130, 143, NA, 119, 104,…
    ## $ h1_sysbp_noninvasive_min    <int> 115, 71, 124, NA, 120, 143, NA, 106, 98, …
    ## $ h1_temp_max                 <dbl> 39.5, 36.3, 36.7, 35.6, NA, 36.7, 36.8, 3…
    ## $ h1_temp_min                 <dbl> 37.5, 36.3, 36.7, 34.8, NA, 36.7, 35.0, 3…
    ## $ d1_albumin_max              <dbl> 2.3, 1.6, NA, NA, NA, NA, NA, NA, 2.7, 3.…
    ## $ d1_albumin_min              <dbl> 2.3, 1.6, NA, NA, NA, NA, NA, NA, 2.7, 3.…
    ## $ d1_bilirubin_max            <dbl> 0.4, 0.5, NA, NA, NA, NA, NA, NA, 0.2, 0.…
    ## $ d1_bilirubin_min            <dbl> 0.4, 0.5, NA, NA, NA, NA, NA, NA, 0.2, 0.…
    ## $ d1_bun_max                  <dbl> 31, 11, NA, NA, NA, 13, 18, 48, 15, 10, 1…
    ## $ d1_bun_min                  <dbl> 30, 9, NA, NA, NA, 13, 11, 48, 15, 10, 14…
    ## $ d1_calcium_max              <dbl> 8.5, 8.6, NA, NA, NA, 8.8, 9.3, 7.8, 7.3,…
    ## $ d1_calcium_min              <dbl> 7.4, 8.0, NA, NA, NA, 8.8, 8.7, 7.8, 7.3,…
    ## $ d1_creatinine_max           <dbl> 2.51, 0.71, NA, NA, NA, 0.71, 0.85, 2.05,…
    ## $ d1_creatinine_min           <dbl> 2.23, 0.56, NA, NA, NA, 0.71, 0.78, 2.05,…
    ## $ d1_glucose_max              <int> 168, 145, NA, 185, NA, 156, 197, 129, 365…
    ## $ d1_glucose_min              <int> 109, 128, NA, 88, NA, 125, 129, 129, 288,…
    ## $ d1_hco3_max                 <dbl> 19, 27, NA, NA, NA, 27, 33, 29, 23, 28, 2…
    ## $ d1_hco3_min                 <dbl> 15, 26, NA, NA, NA, 27, 30, 29, 23, 28, 2…
    ## $ d1_hemaglobin_max           <dbl> 8.9, 11.3, NA, 11.6, NA, 15.6, 11.9, 7.8,…
    ## $ d1_hemaglobin_min           <dbl> 8.9, 11.1, NA, 8.9, NA, 15.6, 10.7, 7.8, …
    ## $ d1_hematocrit_max           <dbl> 27.4, 36.9, NA, 34.0, NA, 44.2, 37.5, 25.…
    ## $ d1_hematocrit_min           <dbl> 27.4, 36.1, NA, 25.9, NA, 44.2, 33.5, 25.…
    ## $ d1_inr_max                  <dbl> NA, 1.3, NA, 1.6, NA, 1.1, NA, NA, 1.2, N…
    ## $ d1_inr_min                  <dbl> NA, 1.3, NA, 1.1, NA, 1.1, NA, NA, 1.2, N…
    ## $ d1_lactate_max              <dbl> 1.3, 3.5, NA, NA, NA, NA, NA, NA, 5.9, NA…
    ## $ d1_lactate_min              <dbl> 1.0, 3.5, NA, NA, NA, NA, NA, NA, 5.9, NA…
    ## $ d1_platelets_max            <int> 233, 557, NA, 198, NA, 159, 295, 260, 226…
    ## $ d1_platelets_min            <dbl> 233, 487, NA, 43, NA, 159, 278, 260, 226,…
    ## $ d1_potassium_max            <dbl> 4.0, 4.2, NA, 5.0, NA, 3.9, 5.0, 5.8, 5.2…
    ## $ d1_potassium_min            <dbl> 3.4, 3.8, NA, 3.5, NA, 3.7, 4.2, 2.4, 5.2…
    ## $ d1_sodium_max               <dbl> 136, 145, NA, NA, NA, 137, 136, 140, 142,…
    ## $ d1_sodium_min               <dbl> 134, 145, NA, NA, NA, 137, 135, 140, 142,…
    ## $ d1_wbc_max                  <dbl> 14.1, 23.3, NA, 9.0, NA, 10.9, 9.3, 12.8,…
    ## $ d1_wbc_min                  <dbl> 14.1, 12.7, NA, 8.0, NA, 10.9, 5.9, 12.8,…
    ## $ h1_albumin_max              <dbl> NA, NA, NA, NA, NA, NA, NA, NA, 2.7, 3.6,…
    ## $ h1_albumin_min              <dbl> NA, NA, NA, NA, NA, NA, NA, NA, 2.7, 3.6,…
    ## $ h1_bilirubin_max            <dbl> NA, NA, NA, NA, NA, NA, NA, NA, 0.2, 0.4,…
    ## $ h1_bilirubin_min            <dbl> NA, NA, NA, NA, NA, NA, NA, NA, 0.2, 0.4,…
    ## $ h1_bun_max                  <dbl> NA, 9, NA, NA, NA, NA, 18, NA, 15, 10, NA…
    ## $ h1_bun_min                  <dbl> NA, 9, NA, NA, NA, NA, 18, NA, 15, 10, NA…
    ## $ h1_calcium_max              <dbl> NA, 8.6, NA, NA, NA, NA, 8.7, NA, 7.3, 8.…
    ## $ h1_calcium_min              <dbl> NA, 8.6, NA, NA, NA, NA, 8.7, NA, 7.3, 8.…
    ## $ h1_creatinine_max           <dbl> NA, 0.56, NA, NA, NA, NA, 0.78, NA, 1.16,…
    ## $ h1_creatinine_min           <dbl> NA, 0.56, NA, NA, NA, NA, 0.78, NA, 1.16,…
    ## $ h1_glucose_max              <dbl> NA, 145, NA, NA, NA, NA, 197, NA, 365, 13…
    ## $ h1_glucose_min              <int> NA, 143, NA, NA, NA, NA, 194, NA, 365, 13…
    ## $ h1_hco3_max                 <dbl> NA, 27, NA, NA, NA, NA, 30, NA, 23, 28, N…
    ## $ h1_hco3_min                 <dbl> NA, 27, NA, NA, NA, NA, 30, NA, 23, 28, N…
    ## $ h1_hemaglobin_max           <dbl> NA, 11.3, NA, 11.6, NA, NA, 10.7, NA, 12.…
    ## $ h1_hemaglobin_min           <dbl> NA, 11.3, NA, 11.6, NA, NA, 10.7, NA, 12.…
    ## $ h1_hematocrit_max           <dbl> NA, 36.9, NA, 34.0, NA, NA, 33.5, NA, 37.…
    ## $ h1_hematocrit_min           <dbl> NA, 36.9, NA, 34.0, NA, NA, 33.5, NA, 37.…
    ## $ h1_inr_max                  <dbl> NA, 1.3, NA, 1.6, NA, 1.1, NA, NA, 1.2, N…
    ## $ h1_inr_min                  <dbl> NA, 1.3, NA, 1.1, NA, 1.1, NA, NA, 1.2, N…
    ## $ h1_lactate_max              <dbl> NA, 3.5, NA, NA, NA, NA, NA, NA, 5.9, NA,…
    ## $ h1_lactate_min              <dbl> NA, 3.5, NA, NA, NA, NA, NA, NA, 5.9, NA,…
    ## $ h1_platelets_max            <int> NA, 557, NA, 43, NA, NA, 278, NA, 226, 23…
    ## $ h1_platelets_min            <int> NA, 557, NA, 43, NA, NA, 278, NA, 226, 23…
    ## $ h1_potassium_max            <dbl> NA, 4.2, NA, NA, NA, NA, 4.2, NA, 5.2, 3.…
    ## $ h1_potassium_min            <dbl> NA, 4.2, NA, NA, NA, NA, 4.2, NA, 5.2, 3.…
    ## $ h1_sodium_max               <dbl> NA, 145, NA, NA, NA, NA, 135, NA, 142, 13…
    ## $ h1_sodium_min               <dbl> NA, 145, NA, NA, NA, NA, 135, NA, 142, 13…
    ## $ h1_wbc_max                  <dbl> NA, 12.7, NA, 8.8, NA, NA, 5.9, NA, 24.7,…
    ## $ h1_wbc_min                  <dbl> NA, 12.7, NA, 8.8, NA, NA, 5.9, NA, 24.7,…
    ## $ d1_arterial_pco2_max        <dbl> NA, 37, NA, 37, NA, NA, 43, 43, 60, NA, N…
    ## $ d1_arterial_pco2_min        <dbl> NA, 37, NA, 27, NA, NA, 43, 43, 33, NA, N…
    ## $ d1_arterial_ph_max          <dbl> NA, 7.45, NA, 7.44, NA, NA, 7.42, 7.38, 7…
    ## $ d1_arterial_ph_min          <dbl> NA, 7.45, NA, 7.34, NA, NA, 7.42, 7.38, 6…
    ## $ d1_arterial_po2_max         <dbl> NA, 51, NA, 337, NA, NA, 370, 89, 256, NA…
    ## $ d1_arterial_po2_min         <dbl> NA, 51, NA, 102, NA, NA, 370, 89, 92, NA,…
    ## $ d1_pao2fio2ratio_max        <dbl> NA, 54.8, NA, 342.5, NA, NA, 370.0, NA, 9…
    ## $ d1_pao2fio2ratio_min        <dbl> NA, 51.0000, NA, 236.6667, NA, NA, 370.00…
    ## $ h1_arterial_pco2_max        <dbl> NA, 37, NA, 36, NA, NA, 43, NA, 60, NA, N…
    ## $ h1_arterial_pco2_min        <dbl> NA, 37, NA, 33, NA, NA, 43, NA, 60, NA, N…
    ## $ h1_arterial_ph_max          <dbl> NA, 7.45, NA, 7.37, NA, NA, 7.42, NA, 7.1…
    ## $ h1_arterial_ph_min          <dbl> NA, 7.45, NA, 7.34, NA, NA, 7.42, NA, 7.1…
    ## $ h1_arterial_po2_max         <dbl> NA, 51, NA, 337, NA, NA, 370, NA, 92, NA,…
    ## $ h1_arterial_po2_min         <dbl> NA, 51, NA, 265, NA, NA, 370, NA, 92, NA,…
    ## $ h1_pao2fio2ratio_max        <dbl> NA, 51.0000, NA, 337.0000, NA, NA, 370.00…
    ## $ h1_pao2fio2ratio_min        <dbl> NA, 51.0000, NA, 337.0000, NA, NA, 370.00…
    ## $ aids                        <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ cirrhosis                   <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ hepatic_failure             <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ immunosuppression           <int> 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0,…
    ## $ leukemia                    <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0,…
    ## $ lymphoma                    <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ solid_tumor_with_metastasis <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ diabetes_mellitus           <int> 1, 1, 0, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0,…

``` r
Diabetes%>%
  filter(!is.na(gender))%>%
  ggplot(aes(x = gender, fill = factor(diabetes_mellitus)))+
  geom_bar(position = "fill")
```

![](wids_dataXB_eda_files/figure-gfm/diabetes_by_gender-1.png)<!-- -->

``` r
Diabetes%>%
  ggplot(aes(x = ethnicity, fill = factor(diabetes_mellitus))) + 
  geom_bar(position = "fill")
```

![](wids_dataXB_eda_files/figure-gfm/diabetes-ethnicity-1.png)<!-- -->

``` r
copyD <- Diabetes%>%
  filter(!is.na(ethnicity))%>%
  mutate(African = ifelse(ethnicity == "African American", 1, 0 ), 
         Caucasian = ifelse(ethnicity == "Caucasian", 1, 0 ), 
         Asian = ifelse(ethnicity == "Asian", 1, 0 ), 
         Hispanic = ifelse(ethnicity == "Hispanic", 1, 0 ), 
         Native_Am = ifelse(ethnicity == "Native American", 1, 0 ), 
         Other = ifelse(ethnicity == "Other/Unknown", 1, 0))

glimpse(copyD)
```

    ## Rows: 128,570
    ## Columns: 187
    ## $ X                           <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 12, 13, 14…
    ## $ encounter_id                <int> 214826, 246060, 276985, 262220, 201746, 1…
    ## $ hospital_id                 <int> 118, 81, 118, 118, 33, 83, 83, 33, 118, 7…
    ## $ age                         <int> 68, 77, 25, 81, 19, 67, 59, 70, 45, 72, 8…
    ## $ bmi                         <dbl> 22.73280, 27.42188, 31.95275, 22.63555, N…
    ## $ elective_surgery            <int> 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 1, 0, 1, 1,…
    ## $ ethnicity                   <chr> "Caucasian", "Caucasian", "Caucasian", "C…
    ## $ gender                      <chr> "M", "F", "F", "F", "M", "M", "F", "M", "…
    ## $ height                      <dbl> 180.3, 160.0, 172.7, 165.1, 188.0, 190.5,…
    ## $ hospital_admit_source       <chr> "Floor", "Floor", "Emergency Department",…
    ## $ icu_admit_source            <chr> "Floor", "Floor", "Accident & Emergency",…
    ## $ icu_id                      <int> 92, 90, 93, 92, 91, 95, 95, 91, 114, 113,…
    ## $ icu_stay_type               <chr> "admit", "admit", "admit", "admit", "admi…
    ## $ icu_type                    <chr> "CTICU", "Med-Surg ICU", "Med-Surg ICU", …
    ## $ pre_icu_los_days            <dbl> 0.541666667, 0.927777778, 0.000694444, 0.…
    ## $ readmission_status          <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ weight                      <dbl> 73.9, 70.2, 95.3, 61.7, NA, 100.0, 156.6,…
    ## $ albumin_apache              <dbl> 2.3, NA, NA, NA, NA, NA, NA, NA, 2.7, NA,…
    ## $ apache_2_diagnosis          <int> 113, 108, 122, 203, 119, 301, 108, 113, 1…
    ## $ apache_3j_diagnosis         <dbl> 502.01, 203.01, 703.03, 1206.03, 601.01, …
    ## $ apache_post_operative       <int> 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 1, 0, 1, 1,…
    ## $ arf_apache                  <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ bilirubin_apache            <dbl> 0.4, NA, NA, NA, NA, NA, NA, NA, 0.2, NA,…
    ## $ bun_apache                  <dbl> 31, 9, NA, NA, NA, 13, 18, 48, 15, NA, 29…
    ## $ creatinine_apache           <dbl> 2.51, 0.56, NA, NA, NA, 0.71, 0.78, 2.05,…
    ## $ fio2_apache                 <dbl> NA, 1.0, NA, 0.6, NA, NA, 1.0, NA, 1.0, N…
    ## $ gcs_eyes_apache             <int> 3, 1, 3, 4, NA, 4, 4, 4, 4, 4, 4, 4, 4, 4…
    ## $ gcs_motor_apache            <int> 6, 3, 6, 6, NA, 6, 6, 6, 6, 6, 6, 6, 6, 6…
    ## $ gcs_unable_apache           <int> 0, 0, 0, 0, NA, 0, 0, 0, 0, 0, 0, 0, 0, 0…
    ## $ gcs_verbal_apache           <int> 4, 1, 5, 5, NA, 5, 5, 5, 5, 5, 4, 5, 5, 5…
    ## $ glucose_apache              <dbl> 168, 145, NA, 185, NA, 156, 197, 164, 380…
    ## $ heart_rate_apache           <int> 118, 120, 102, 114, 60, 113, 133, 120, 82…
    ## $ hematocrit_apache           <dbl> 27.4, 36.9, NA, 25.9, NA, 44.2, 33.5, 22.…
    ## $ intubated_apache            <int> 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0,…
    ## $ map_apache                  <dbl> 40, 46, 68, 60, 103, 130, 138, 60, 66, 72…
    ## $ paco2_apache                <dbl> NA, 37, NA, 30, NA, NA, 43, NA, 60, NA, N…
    ## $ paco2_for_ph_apache         <dbl> NA, 37, NA, 30, NA, NA, 43, NA, 60, NA, N…
    ## $ pao2_apache                 <dbl> NA, 51, NA, 142, NA, NA, 370, NA, 92, NA,…
    ## $ ph_apache                   <dbl> NA, 7.45, NA, 7.39, NA, NA, 7.42, NA, 7.1…
    ## $ resprate_apache             <dbl> 36, 33, 37, 4, 16, 35, 53, 28, 14, 15, 24…
    ## $ sodium_apache               <dbl> 134, 145, NA, NA, NA, 137, 135, 140, 142,…
    ## $ temp_apache                 <dbl> 39.3, 35.1, 36.7, 34.8, 36.7, 36.6, 35.0,…
    ## $ urineoutput_apache          <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    ## $ ventilated_apache           <int> 0, 1, 0, 1, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0,…
    ## $ wbc_apache                  <dbl> 14.1, 12.7, NA, 8.0, NA, 10.9, 5.9, 12.8,…
    ## $ d1_diasbp_invasive_max      <int> 46, NA, NA, 62, NA, NA, 107, NA, 64, 80, …
    ## $ d1_diasbp_invasive_min      <int> 32, NA, NA, 30, NA, NA, 65, NA, 52, 51, 4…
    ## $ d1_diasbp_max               <int> 68, 95, 88, 48, 99, 100, 76, 84, 65, 72, …
    ## $ d1_diasbp_min               <int> 37, 31, 48, 42, 57, 61, 68, 46, 59, 53, 8…
    ## $ d1_diasbp_noninvasive_max   <int> 68, 95, 88, 48, 99, 100, 76, 84, 65, 72, …
    ## $ d1_diasbp_noninvasive_min   <int> 37, 31, 48, 42, 57, 61, 68, 46, 59, 53, 8…
    ## $ d1_heartrate_max            <int> 119, 118, 96, 116, 89, 113, 112, 118, 82,…
    ## $ d1_heartrate_min            <int> 72, 72, 68, 92, 60, 83, 70, 86, 82, 67, 5…
    ## $ d1_mbp_invasive_max         <int> 66, NA, NA, 92, NA, NA, 138, NA, 72, 105,…
    ## $ d1_mbp_invasive_min         <int> 40, NA, NA, 52, NA, NA, 84, NA, 66, 69, 6…
    ## $ d1_mbp_max                  <dbl> 89, 120, 102, 84, 104, 127, 117, 114, 93,…
    ## $ d1_mbp_min                  <dbl> 46, 38, 68, 84, 90, 80, 97, 60, 71, 70, 1…
    ## $ d1_mbp_noninvasive_max      <dbl> 89, 120, 102, 84, 104, 127, 117, 114, 93,…
    ## $ d1_mbp_noninvasive_min      <dbl> 46, 38, 68, 84, 90, 80, 97, 60, 71, 70, 1…
    ## $ d1_resprate_max             <int> 34, 32, 21, 23, 18, 32, 38, 28, 24, 23, 2…
    ## $ d1_resprate_min             <int> 10, 12, 8, 7, 16, 10, 16, 12, 19, 14, 16,…
    ## $ d1_spo2_max                 <int> 100, 100, 98, 100, 100, 97, 100, 100, 97,…
    ## $ d1_spo2_min                 <int> 74, 70, 91, 95, 96, 91, 87, 92, 97, 92, 7…
    ## $ d1_sysbp_invasive_max       <int> 122, NA, NA, 164, NA, NA, 191, NA, 94, 16…
    ## $ d1_sysbp_invasive_min       <int> 64, NA, NA, 78, NA, NA, 116, NA, 72, 101,…
    ## $ d1_sysbp_max                <int> 131, 159, 148, 158, 147, 173, 151, 147, 1…
    ## $ d1_sysbp_min                <int> 73, 67, 105, 84, 120, 107, 133, 71, 98, 9…
    ## $ d1_sysbp_noninvasive_max    <int> 131, 159, 148, 158, 147, 173, 151, 147, 1…
    ## $ d1_sysbp_noninvasive_min    <dbl> 73, 67, 105, 84, 120, 107, 133, 71, 98, 9…
    ## $ d1_temp_max                 <dbl> 39.9, 36.3, 37.0, 38.0, 37.2, 36.8, 37.2,…
    ## $ d1_temp_min                 <dbl> 37.2, 35.1, 36.7, 34.8, 36.7, 36.6, 35.0,…
    ## $ h1_diasbp_invasive_max      <int> NA, NA, NA, 62, NA, NA, 107, NA, 64, 74, …
    ## $ h1_diasbp_invasive_min      <int> NA, NA, NA, 44, NA, NA, 79, NA, 52, 64, 4…
    ## $ h1_diasbp_max               <int> 68, 61, 88, 62, 99, 89, 107, 74, 65, 72, …
    ## $ h1_diasbp_min               <int> 63, 48, 58, 44, 68, 89, 79, 55, 59, 56, 8…
    ## $ h1_diasbp_noninvasive_max   <int> 68, 61, 88, NA, 99, 89, NA, 74, 65, 72, 8…
    ## $ h1_diasbp_noninvasive_min   <int> 63, 48, 58, NA, 68, 89, NA, 55, 59, 56, 8…
    ## $ h1_heartrate_max            <int> 119, 114, 96, 100, 89, 83, 79, 118, 82, 9…
    ## $ h1_heartrate_min            <int> 108, 100, 78, 96, 76, 83, 72, 114, 82, 70…
    ## $ h1_mbp_invasive_max         <dbl> NA, NA, NA, 92, NA, NA, 138, NA, 72, 100,…
    ## $ h1_mbp_invasive_min         <int> NA, NA, NA, 71, NA, NA, 115, NA, 66, 89, …
    ## $ h1_mbp_max                  <dbl> 86, 85, 91, 92, 104, 111, 117, 88, 93, 91…
    ## $ h1_mbp_min                  <dbl> 85, 57, 83, 71, 92, 111, 117, 60, 71, 87,…
    ## $ h1_mbp_noninvasive_max      <dbl> 86, 85, 91, NA, 104, 111, 117, 88, 93, 91…
    ## $ h1_mbp_noninvasive_min      <dbl> 85, 57, 83, NA, 92, 111, 117, 60, 71, 87,…
    ## $ h1_resprate_max             <int> 26, 31, 20, 12, NA, 12, 18, 28, 24, 23, N…
    ## $ h1_resprate_min             <int> 18, 28, 16, 11, NA, 12, 18, 26, 19, 14, N…
    ## $ h1_spo2_max                 <int> 100, 95, 98, 100, 100, 97, 100, 96, 97, 9…
    ## $ h1_spo2_min                 <int> 74, 70, 91, 99, 100, 97, 100, 92, 97, 93,…
    ## $ h1_sysbp_invasive_max       <int> NA, NA, NA, 136, NA, NA, 191, NA, 94, 168…
    ## $ h1_sysbp_invasive_min       <dbl> NA, NA, NA, 106, NA, NA, 163, NA, 72, 136…
    ## $ h1_sysbp_max                <int> 131, 95, 148, 136, 130, 143, 191, 119, 10…
    ## $ h1_sysbp_min                <int> 115, 71, 124, 106, 120, 143, 163, 106, 98…
    ## $ h1_sysbp_noninvasive_max    <int> 131, 95, 148, NA, 130, 143, NA, 119, 104,…
    ## $ h1_sysbp_noninvasive_min    <int> 115, 71, 124, NA, 120, 143, NA, 106, 98, …
    ## $ h1_temp_max                 <dbl> 39.5, 36.3, 36.7, 35.6, NA, 36.7, 36.8, 3…
    ## $ h1_temp_min                 <dbl> 37.5, 36.3, 36.7, 34.8, NA, 36.7, 35.0, 3…
    ## $ d1_albumin_max              <dbl> 2.3, 1.6, NA, NA, NA, NA, NA, NA, 2.7, NA…
    ## $ d1_albumin_min              <dbl> 2.3, 1.6, NA, NA, NA, NA, NA, NA, 2.7, NA…
    ## $ d1_bilirubin_max            <dbl> 0.4, 0.5, NA, NA, NA, NA, NA, NA, 0.2, NA…
    ## $ d1_bilirubin_min            <dbl> 0.4, 0.5, NA, NA, NA, NA, NA, NA, 0.2, NA…
    ## $ d1_bun_max                  <dbl> 31, 11, NA, NA, NA, 13, 18, 48, 15, 14, 2…
    ## $ d1_bun_min                  <dbl> 30, 9, NA, NA, NA, 13, 11, 48, 15, 14, 29…
    ## $ d1_calcium_max              <dbl> 8.5, 8.6, NA, NA, NA, 8.8, 9.3, 7.8, 7.3,…
    ## $ d1_calcium_min              <dbl> 7.4, 8.0, NA, NA, NA, 8.8, 8.7, 7.8, 7.3,…
    ## $ d1_creatinine_max           <dbl> 2.51, 0.71, NA, NA, NA, 0.71, 0.85, 2.05,…
    ## $ d1_creatinine_min           <dbl> 2.23, 0.56, NA, NA, NA, 0.71, 0.78, 2.05,…
    ## $ d1_glucose_max              <int> 168, 145, NA, 185, NA, 156, 197, 129, 365…
    ## $ d1_glucose_min              <int> 109, 128, NA, 88, NA, 125, 129, 129, 288,…
    ## $ d1_hco3_max                 <dbl> 19, 27, NA, NA, NA, 27, 33, 29, 23, 26, 2…
    ## $ d1_hco3_min                 <dbl> 15, 26, NA, NA, NA, 27, 30, 29, 23, 26, 1…
    ## $ d1_hemaglobin_max           <dbl> 8.9, 11.3, NA, 11.6, NA, 15.6, 11.9, 7.8,…
    ## $ d1_hemaglobin_min           <dbl> 8.9, 11.1, NA, 8.9, NA, 15.6, 10.7, 7.8, …
    ## $ d1_hematocrit_max           <dbl> 27.4, 36.9, NA, 34.0, NA, 44.2, 37.5, 25.…
    ## $ d1_hematocrit_min           <dbl> 27.4, 36.1, NA, 25.9, NA, 44.2, 33.5, 25.…
    ## $ d1_inr_max                  <dbl> NA, 1.3, NA, 1.6, NA, 1.1, NA, NA, 1.2, N…
    ## $ d1_inr_min                  <dbl> NA, 1.3, NA, 1.1, NA, 1.1, NA, NA, 1.2, N…
    ## $ d1_lactate_max              <dbl> 1.3, 3.5, NA, NA, NA, NA, NA, NA, 5.9, NA…
    ## $ d1_lactate_min              <dbl> 1.0, 3.5, NA, NA, NA, NA, NA, NA, 5.9, NA…
    ## $ d1_platelets_max            <int> 233, 557, NA, 198, NA, 159, 295, 260, 226…
    ## $ d1_platelets_min            <dbl> 233, 487, NA, 43, NA, 159, 278, 260, 226,…
    ## $ d1_potassium_max            <dbl> 4.0, 4.2, NA, 5.0, NA, 3.9, 5.0, 5.8, 5.2…
    ## $ d1_potassium_min            <dbl> 3.4, 3.8, NA, 3.5, NA, 3.7, 4.2, 2.4, 5.2…
    ## $ d1_sodium_max               <dbl> 136, 145, NA, NA, NA, 137, 136, 140, 142,…
    ## $ d1_sodium_min               <dbl> 134, 145, NA, NA, NA, 137, 135, 140, 142,…
    ## $ d1_wbc_max                  <dbl> 14.1, 23.3, NA, 9.0, NA, 10.9, 9.3, 12.8,…
    ## $ d1_wbc_min                  <dbl> 14.1, 12.7, NA, 8.0, NA, 10.9, 5.9, 12.8,…
    ## $ h1_albumin_max              <dbl> NA, NA, NA, NA, NA, NA, NA, NA, 2.7, NA, …
    ## $ h1_albumin_min              <dbl> NA, NA, NA, NA, NA, NA, NA, NA, 2.7, NA, …
    ## $ h1_bilirubin_max            <dbl> NA, NA, NA, NA, NA, NA, NA, NA, 0.2, NA, …
    ## $ h1_bilirubin_min            <dbl> NA, NA, NA, NA, NA, NA, NA, NA, 0.2, NA, …
    ## $ h1_bun_max                  <dbl> NA, 9, NA, NA, NA, NA, 18, NA, 15, NA, NA…
    ## $ h1_bun_min                  <dbl> NA, 9, NA, NA, NA, NA, 18, NA, 15, NA, NA…
    ## $ h1_calcium_max              <dbl> NA, 8.6, NA, NA, NA, NA, 8.7, NA, 7.3, NA…
    ## $ h1_calcium_min              <dbl> NA, 8.6, NA, NA, NA, NA, 8.7, NA, 7.3, NA…
    ## $ h1_creatinine_max           <dbl> NA, 0.56, NA, NA, NA, NA, 0.78, NA, 1.16,…
    ## $ h1_creatinine_min           <dbl> NA, 0.56, NA, NA, NA, NA, 0.78, NA, 1.16,…
    ## $ h1_glucose_max              <dbl> NA, 145, NA, NA, NA, NA, 197, NA, 365, 15…
    ## $ h1_glucose_min              <int> NA, 143, NA, NA, NA, NA, 194, NA, 365, 15…
    ## $ h1_hco3_max                 <dbl> NA, 27, NA, NA, NA, NA, 30, NA, 23, NA, N…
    ## $ h1_hco3_min                 <dbl> NA, 27, NA, NA, NA, NA, 30, NA, 23, NA, N…
    ## $ h1_hemaglobin_max           <dbl> NA, 11.3, NA, 11.6, NA, NA, 10.7, NA, 12.…
    ## $ h1_hemaglobin_min           <dbl> NA, 11.3, NA, 11.6, NA, NA, 10.7, NA, 12.…
    ## $ h1_hematocrit_max           <dbl> NA, 36.9, NA, 34.0, NA, NA, 33.5, NA, 37.…
    ## $ h1_hematocrit_min           <dbl> NA, 36.9, NA, 34.0, NA, NA, 33.5, NA, 37.…
    ## $ h1_inr_max                  <dbl> NA, 1.3, NA, 1.6, NA, 1.1, NA, NA, 1.2, N…
    ## $ h1_inr_min                  <dbl> NA, 1.3, NA, 1.1, NA, 1.1, NA, NA, 1.2, N…
    ## $ h1_lactate_max              <dbl> NA, 3.5, NA, NA, NA, NA, NA, NA, 5.9, NA,…
    ## $ h1_lactate_min              <dbl> NA, 3.5, NA, NA, NA, NA, NA, NA, 5.9, NA,…
    ## $ h1_platelets_max            <int> NA, 557, NA, 43, NA, NA, 278, NA, 226, NA…
    ## $ h1_platelets_min            <int> NA, 557, NA, 43, NA, NA, 278, NA, 226, NA…
    ## $ h1_potassium_max            <dbl> NA, 4.2, NA, NA, NA, NA, 4.2, NA, 5.2, NA…
    ## $ h1_potassium_min            <dbl> NA, 4.2, NA, NA, NA, NA, 4.2, NA, 5.2, NA…
    ## $ h1_sodium_max               <dbl> NA, 145, NA, NA, NA, NA, 135, NA, 142, NA…
    ## $ h1_sodium_min               <dbl> NA, 145, NA, NA, NA, NA, 135, NA, 142, NA…
    ## $ h1_wbc_max                  <dbl> NA, 12.7, NA, 8.8, NA, NA, 5.9, NA, 24.7,…
    ## $ h1_wbc_min                  <dbl> NA, 12.7, NA, 8.8, NA, NA, 5.9, NA, 24.7,…
    ## $ d1_arterial_pco2_max        <dbl> NA, 37, NA, 37, NA, NA, 43, 43, 60, NA, N…
    ## $ d1_arterial_pco2_min        <dbl> NA, 37, NA, 27, NA, NA, 43, 43, 33, NA, N…
    ## $ d1_arterial_ph_max          <dbl> NA, 7.45, NA, 7.44, NA, NA, 7.42, 7.38, 7…
    ## $ d1_arterial_ph_min          <dbl> NA, 7.45, NA, 7.34, NA, NA, 7.42, 7.38, 6…
    ## $ d1_arterial_po2_max         <dbl> NA, 51, NA, 337, NA, NA, 370, 89, 256, NA…
    ## $ d1_arterial_po2_min         <dbl> NA, 51, NA, 102, NA, NA, 370, 89, 92, NA,…
    ## $ d1_pao2fio2ratio_max        <dbl> NA, 54.8, NA, 342.5, NA, NA, 370.0, NA, 9…
    ## $ d1_pao2fio2ratio_min        <dbl> NA, 51.0000, NA, 236.6667, NA, NA, 370.00…
    ## $ h1_arterial_pco2_max        <dbl> NA, 37, NA, 36, NA, NA, 43, NA, 60, NA, N…
    ## $ h1_arterial_pco2_min        <dbl> NA, 37, NA, 33, NA, NA, 43, NA, 60, NA, N…
    ## $ h1_arterial_ph_max          <dbl> NA, 7.45, NA, 7.37, NA, NA, 7.42, NA, 7.1…
    ## $ h1_arterial_ph_min          <dbl> NA, 7.45, NA, 7.34, NA, NA, 7.42, NA, 7.1…
    ## $ h1_arterial_po2_max         <dbl> NA, 51, NA, 337, NA, NA, 370, NA, 92, NA,…
    ## $ h1_arterial_po2_min         <dbl> NA, 51, NA, 265, NA, NA, 370, NA, 92, NA,…
    ## $ h1_pao2fio2ratio_max        <dbl> NA, 51, NA, 337, NA, NA, 370, NA, 92, NA,…
    ## $ h1_pao2fio2ratio_min        <dbl> NA, 51, NA, 337, NA, NA, 370, NA, 92, NA,…
    ## $ aids                        <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ cirrhosis                   <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ hepatic_failure             <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ immunosuppression           <int> 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0,…
    ## $ leukemia                    <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0,…
    ## $ lymphoma                    <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ solid_tumor_with_metastasis <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ diabetes_mellitus           <int> 1, 1, 0, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0,…
    ## $ African                     <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ Caucasian                   <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1,…
    ## $ Asian                       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ Hispanic                    <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0,…
    ## $ Native_Am                   <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ Other                       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…

**Percentage of NAs in each Column** It seems that many columns have NA
percentage greater 70%, should we drop these columns when we train the
model?

``` r
colMeans(is.na(Diabetes))%>%
  kable()
```

|                                |         x |
| :----------------------------- | --------: |
| X                              | 0.0000000 |
| encounter\_id                  | 0.0000000 |
| hospital\_id                   | 0.0000000 |
| age                            | 0.0383229 |
| bmi                            | 0.0344968 |
| elective\_surgery              | 0.0000000 |
| ethnicity                      | 0.0121930 |
| gender                         | 0.0005071 |
| height                         | 0.0159577 |
| hospital\_admit\_source        | 0.2550612 |
| icu\_admit\_source             | 0.0018439 |
| icu\_id                        | 0.0000000 |
| icu\_stay\_type                | 0.0000000 |
| icu\_type                      | 0.0000000 |
| pre\_icu\_los\_days            | 0.0000000 |
| readmission\_status            | 0.0000000 |
| weight                         | 0.0266063 |
| albumin\_apache                | 0.6005286 |
| apache\_2\_diagnosis           | 0.0129459 |
| apache\_3j\_diagnosis          | 0.0066458 |
| apache\_post\_operative        | 0.0000000 |
| arf\_apache                    | 0.0000000 |
| bilirubin\_apache              | 0.6343109 |
| bun\_apache                    | 0.1952334 |
| creatinine\_apache             | 0.1911691 |
| fio2\_apache                   | 0.7661516 |
| gcs\_eyes\_apache              | 0.0168258 |
| gcs\_motor\_apache             | 0.0168258 |
| gcs\_unable\_apache            | 0.0054473 |
| gcs\_verbal\_apache            | 0.0168258 |
| glucose\_apache                | 0.1129098 |
| heart\_rate\_apache            | 0.0023741 |
| hematocrit\_apache             | 0.2055825 |
| intubated\_apache              | 0.0000000 |
| map\_apache                    | 0.0032269 |
| paco2\_apache                  | 0.7661516 |
| paco2\_for\_ph\_apache         | 0.7661516 |
| pao2\_apache                   | 0.7661516 |
| ph\_apache                     | 0.7661516 |
| resprate\_apache               | 0.0062079 |
| sodium\_apache                 | 0.1883802 |
| temp\_apache                   | 0.0507925 |
| urineoutput\_apache            | 0.4853139 |
| ventilated\_apache             | 0.0000000 |
| wbc\_apache                    | 0.2264573 |
| d1\_diasbp\_invasive\_max      | 0.7304102 |
| d1\_diasbp\_invasive\_min      | 0.7304102 |
| d1\_diasbp\_max                | 0.0021282 |
| d1\_diasbp\_min                | 0.0021282 |
| d1\_diasbp\_noninvasive\_max   | 0.0125694 |
| d1\_diasbp\_noninvasive\_min   | 0.0125694 |
| d1\_heartrate\_max             | 0.0020130 |
| d1\_heartrate\_min             | 0.0020130 |
| d1\_mbp\_invasive\_max         | 0.7288736 |
| d1\_mbp\_invasive\_min         | 0.7288736 |
| d1\_mbp\_max                   | 0.0025124 |
| d1\_mbp\_min                   | 0.0025124 |
| d1\_mbp\_noninvasive\_max      | 0.0171178 |
| d1\_mbp\_noninvasive\_min      | 0.0171178 |
| d1\_resprate\_max              | 0.0052475 |
| d1\_resprate\_min              | 0.0052475 |
| d1\_spo2\_max                  | 0.0040874 |
| d1\_spo2\_min                  | 0.0040874 |
| d1\_sysbp\_invasive\_max       | 0.7301797 |
| d1\_sysbp\_invasive\_min       | 0.7301797 |
| d1\_sysbp\_max                 | 0.0020821 |
| d1\_sysbp\_min                 | 0.0020821 |
| d1\_sysbp\_noninvasive\_max    | 0.0124696 |
| d1\_sysbp\_noninvasive\_min    | 0.0124696 |
| d1\_temp\_max                  | 0.0345275 |
| d1\_temp\_min                  | 0.0345275 |
| h1\_diasbp\_invasive\_max      | 0.8054042 |
| h1\_diasbp\_invasive\_min      | 0.8054042 |
| h1\_diasbp\_max                | 0.0424641 |
| h1\_diasbp\_min                | 0.0424641 |
| h1\_diasbp\_noninvasive\_max   | 0.0871179 |
| h1\_diasbp\_noninvasive\_min   | 0.0871179 |
| h1\_heartrate\_max             | 0.0313007 |
| h1\_heartrate\_min             | 0.0313007 |
| h1\_mbp\_invasive\_max         | 0.8049202 |
| h1\_mbp\_invasive\_min         | 0.8049202 |
| h1\_mbp\_max                   | 0.0501702 |
| h1\_mbp\_min                   | 0.0501702 |
| h1\_mbp\_noninvasive\_max      | 0.1021612 |
| h1\_mbp\_noninvasive\_min      | 0.1021612 |
| h1\_resprate\_max              | 0.0495863 |
| h1\_resprate\_min              | 0.0495863 |
| h1\_spo2\_max                  | 0.0479575 |
| h1\_spo2\_min                  | 0.0479575 |
| h1\_sysbp\_invasive\_max       | 0.8052352 |
| h1\_sysbp\_invasive\_min       | 0.8052352 |
| h1\_sysbp\_max                 | 0.0424026 |
| h1\_sysbp\_min                 | 0.0424026 |
| h1\_sysbp\_noninvasive\_max    | 0.0870487 |
| h1\_sysbp\_noninvasive\_min    | 0.0870487 |
| h1\_temp\_max                  | 0.2282090 |
| h1\_temp\_min                  | 0.2282090 |
| d1\_albumin\_max               | 0.5486144 |
| d1\_albumin\_min               | 0.5486144 |
| d1\_bilirubin\_max             | 0.5895572 |
| d1\_bilirubin\_min             | 0.5895572 |
| d1\_bun\_max                   | 0.1055187 |
| d1\_bun\_min                   | 0.1055187 |
| d1\_calcium\_max               | 0.1282451 |
| d1\_calcium\_min               | 0.1282451 |
| d1\_creatinine\_max            | 0.1019768 |
| d1\_creatinine\_min            | 0.1019768 |
| d1\_glucose\_max               | 0.0633312 |
| d1\_glucose\_min               | 0.0633312 |
| d1\_hco3\_max                  | 0.1540217 |
| d1\_hco3\_min                  | 0.1540217 |
| d1\_hemaglobin\_max            | 0.1247109 |
| d1\_hemaglobin\_min            | 0.1247109 |
| d1\_hematocrit\_max            | 0.1197631 |
| d1\_hematocrit\_min            | 0.1197631 |
| d1\_inr\_max                   | 0.6239618 |
| d1\_inr\_min                   | 0.6239618 |
| d1\_lactate\_max               | 0.7337523 |
| d1\_lactate\_min               | 0.7337523 |
| d1\_platelets\_max             | 0.1425740 |
| d1\_platelets\_min             | 0.1425740 |
| d1\_potassium\_max             | 0.0963913 |
| d1\_potassium\_min             | 0.0963913 |
| d1\_sodium\_max                | 0.1019538 |
| d1\_sodium\_min                | 0.1019538 |
| d1\_wbc\_max                   | 0.1339075 |
| d1\_wbc\_min                   | 0.1339075 |
| h1\_albumin\_max               | 0.9143189 |
| h1\_albumin\_min               | 0.9143189 |
| h1\_bilirubin\_max             | 0.9208955 |
| h1\_bilirubin\_min             | 0.9208955 |
| h1\_bun\_max                   | 0.8066412 |
| h1\_bun\_min                   | 0.8066412 |
| h1\_calcium\_max               | 0.8137941 |
| h1\_calcium\_min               | 0.8137941 |
| h1\_creatinine\_max            | 0.8050585 |
| h1\_creatinine\_min            | 0.8050585 |
| h1\_glucose\_max               | 0.5767880 |
| h1\_glucose\_min               | 0.5767880 |
| h1\_hco3\_max                  | 0.8174359 |
| h1\_hco3\_min                  | 0.8174359 |
| h1\_hemaglobin\_max            | 0.7897385 |
| h1\_hemaglobin\_min            | 0.7897385 |
| h1\_hematocrit\_max            | 0.7910139 |
| h1\_hematocrit\_min            | 0.7910139 |
| h1\_inr\_max                   | 0.6239618 |
| h1\_inr\_min                   | 0.6239618 |
| h1\_lactate\_max               | 0.9101854 |
| h1\_lactate\_min               | 0.9101854 |
| h1\_platelets\_max             | 0.8123190 |
| h1\_platelets\_min             | 0.8123190 |
| h1\_potassium\_max             | 0.7746107 |
| h1\_potassium\_min             | 0.7746107 |
| h1\_sodium\_max                | 0.7819864 |
| h1\_sodium\_min                | 0.7819864 |
| h1\_wbc\_max                   | 0.8142935 |
| h1\_wbc\_min                   | 0.8142935 |
| d1\_arterial\_pco2\_max        | 0.6489163 |
| d1\_arterial\_pco2\_min        | 0.6489163 |
| d1\_arterial\_ph\_max          | 0.6515746 |
| d1\_arterial\_ph\_min          | 0.6515746 |
| d1\_arterial\_po2\_max         | 0.6454513 |
| d1\_arterial\_po2\_min         | 0.6454513 |
| d1\_pao2fio2ratio\_max         | 0.7171262 |
| d1\_pao2fio2ratio\_min         | 0.7171262 |
| h1\_arterial\_pco2\_max        | 0.8272010 |
| h1\_arterial\_pco2\_min        | 0.8272010 |
| h1\_arterial\_ph\_max          | 0.8286070 |
| h1\_arterial\_ph\_min          | 0.8286070 |
| h1\_arterial\_po2\_max         | 0.8255030 |
| h1\_arterial\_po2\_min         | 0.8255030 |
| h1\_pao2fio2ratio\_max         | 0.8712324 |
| h1\_pao2fio2ratio\_min         | 0.8712324 |
| aids                           | 0.0000000 |
| cirrhosis                      | 0.0000000 |
| hepatic\_failure               | 0.0000000 |
| immunosuppression              | 0.0000000 |
| leukemia                       | 0.0000000 |
| lymphoma                       | 0.0000000 |
| solid\_tumor\_with\_metastasis | 0.0000000 |
| diabetes\_mellitus             | 0.0000000 |

``` r
#this function was created as a review on how to write function in R. Has no 
#actual meaning to this dataset
col_classes <- function(df) {
    lapply(df, function(x) paste(class(x), collapse = ','))
}

col_classes(Diabetes)
```

    ## $X
    ## [1] "integer"
    ## 
    ## $encounter_id
    ## [1] "integer"
    ## 
    ## $hospital_id
    ## [1] "integer"
    ## 
    ## $age
    ## [1] "integer"
    ## 
    ## $bmi
    ## [1] "numeric"
    ## 
    ## $elective_surgery
    ## [1] "integer"
    ## 
    ## $ethnicity
    ## [1] "character"
    ## 
    ## $gender
    ## [1] "character"
    ## 
    ## $height
    ## [1] "numeric"
    ## 
    ## $hospital_admit_source
    ## [1] "character"
    ## 
    ## $icu_admit_source
    ## [1] "character"
    ## 
    ## $icu_id
    ## [1] "integer"
    ## 
    ## $icu_stay_type
    ## [1] "character"
    ## 
    ## $icu_type
    ## [1] "character"
    ## 
    ## $pre_icu_los_days
    ## [1] "numeric"
    ## 
    ## $readmission_status
    ## [1] "integer"
    ## 
    ## $weight
    ## [1] "numeric"
    ## 
    ## $albumin_apache
    ## [1] "numeric"
    ## 
    ## $apache_2_diagnosis
    ## [1] "integer"
    ## 
    ## $apache_3j_diagnosis
    ## [1] "numeric"
    ## 
    ## $apache_post_operative
    ## [1] "integer"
    ## 
    ## $arf_apache
    ## [1] "integer"
    ## 
    ## $bilirubin_apache
    ## [1] "numeric"
    ## 
    ## $bun_apache
    ## [1] "numeric"
    ## 
    ## $creatinine_apache
    ## [1] "numeric"
    ## 
    ## $fio2_apache
    ## [1] "numeric"
    ## 
    ## $gcs_eyes_apache
    ## [1] "integer"
    ## 
    ## $gcs_motor_apache
    ## [1] "integer"
    ## 
    ## $gcs_unable_apache
    ## [1] "integer"
    ## 
    ## $gcs_verbal_apache
    ## [1] "integer"
    ## 
    ## $glucose_apache
    ## [1] "numeric"
    ## 
    ## $heart_rate_apache
    ## [1] "integer"
    ## 
    ## $hematocrit_apache
    ## [1] "numeric"
    ## 
    ## $intubated_apache
    ## [1] "integer"
    ## 
    ## $map_apache
    ## [1] "numeric"
    ## 
    ## $paco2_apache
    ## [1] "numeric"
    ## 
    ## $paco2_for_ph_apache
    ## [1] "numeric"
    ## 
    ## $pao2_apache
    ## [1] "numeric"
    ## 
    ## $ph_apache
    ## [1] "numeric"
    ## 
    ## $resprate_apache
    ## [1] "numeric"
    ## 
    ## $sodium_apache
    ## [1] "numeric"
    ## 
    ## $temp_apache
    ## [1] "numeric"
    ## 
    ## $urineoutput_apache
    ## [1] "numeric"
    ## 
    ## $ventilated_apache
    ## [1] "integer"
    ## 
    ## $wbc_apache
    ## [1] "numeric"
    ## 
    ## $d1_diasbp_invasive_max
    ## [1] "integer"
    ## 
    ## $d1_diasbp_invasive_min
    ## [1] "integer"
    ## 
    ## $d1_diasbp_max
    ## [1] "integer"
    ## 
    ## $d1_diasbp_min
    ## [1] "integer"
    ## 
    ## $d1_diasbp_noninvasive_max
    ## [1] "integer"
    ## 
    ## $d1_diasbp_noninvasive_min
    ## [1] "integer"
    ## 
    ## $d1_heartrate_max
    ## [1] "integer"
    ## 
    ## $d1_heartrate_min
    ## [1] "integer"
    ## 
    ## $d1_mbp_invasive_max
    ## [1] "integer"
    ## 
    ## $d1_mbp_invasive_min
    ## [1] "integer"
    ## 
    ## $d1_mbp_max
    ## [1] "numeric"
    ## 
    ## $d1_mbp_min
    ## [1] "numeric"
    ## 
    ## $d1_mbp_noninvasive_max
    ## [1] "numeric"
    ## 
    ## $d1_mbp_noninvasive_min
    ## [1] "numeric"
    ## 
    ## $d1_resprate_max
    ## [1] "integer"
    ## 
    ## $d1_resprate_min
    ## [1] "integer"
    ## 
    ## $d1_spo2_max
    ## [1] "integer"
    ## 
    ## $d1_spo2_min
    ## [1] "integer"
    ## 
    ## $d1_sysbp_invasive_max
    ## [1] "integer"
    ## 
    ## $d1_sysbp_invasive_min
    ## [1] "integer"
    ## 
    ## $d1_sysbp_max
    ## [1] "integer"
    ## 
    ## $d1_sysbp_min
    ## [1] "integer"
    ## 
    ## $d1_sysbp_noninvasive_max
    ## [1] "integer"
    ## 
    ## $d1_sysbp_noninvasive_min
    ## [1] "numeric"
    ## 
    ## $d1_temp_max
    ## [1] "numeric"
    ## 
    ## $d1_temp_min
    ## [1] "numeric"
    ## 
    ## $h1_diasbp_invasive_max
    ## [1] "integer"
    ## 
    ## $h1_diasbp_invasive_min
    ## [1] "integer"
    ## 
    ## $h1_diasbp_max
    ## [1] "integer"
    ## 
    ## $h1_diasbp_min
    ## [1] "integer"
    ## 
    ## $h1_diasbp_noninvasive_max
    ## [1] "integer"
    ## 
    ## $h1_diasbp_noninvasive_min
    ## [1] "integer"
    ## 
    ## $h1_heartrate_max
    ## [1] "integer"
    ## 
    ## $h1_heartrate_min
    ## [1] "integer"
    ## 
    ## $h1_mbp_invasive_max
    ## [1] "numeric"
    ## 
    ## $h1_mbp_invasive_min
    ## [1] "integer"
    ## 
    ## $h1_mbp_max
    ## [1] "numeric"
    ## 
    ## $h1_mbp_min
    ## [1] "numeric"
    ## 
    ## $h1_mbp_noninvasive_max
    ## [1] "numeric"
    ## 
    ## $h1_mbp_noninvasive_min
    ## [1] "numeric"
    ## 
    ## $h1_resprate_max
    ## [1] "integer"
    ## 
    ## $h1_resprate_min
    ## [1] "integer"
    ## 
    ## $h1_spo2_max
    ## [1] "integer"
    ## 
    ## $h1_spo2_min
    ## [1] "integer"
    ## 
    ## $h1_sysbp_invasive_max
    ## [1] "integer"
    ## 
    ## $h1_sysbp_invasive_min
    ## [1] "numeric"
    ## 
    ## $h1_sysbp_max
    ## [1] "integer"
    ## 
    ## $h1_sysbp_min
    ## [1] "integer"
    ## 
    ## $h1_sysbp_noninvasive_max
    ## [1] "integer"
    ## 
    ## $h1_sysbp_noninvasive_min
    ## [1] "integer"
    ## 
    ## $h1_temp_max
    ## [1] "numeric"
    ## 
    ## $h1_temp_min
    ## [1] "numeric"
    ## 
    ## $d1_albumin_max
    ## [1] "numeric"
    ## 
    ## $d1_albumin_min
    ## [1] "numeric"
    ## 
    ## $d1_bilirubin_max
    ## [1] "numeric"
    ## 
    ## $d1_bilirubin_min
    ## [1] "numeric"
    ## 
    ## $d1_bun_max
    ## [1] "numeric"
    ## 
    ## $d1_bun_min
    ## [1] "numeric"
    ## 
    ## $d1_calcium_max
    ## [1] "numeric"
    ## 
    ## $d1_calcium_min
    ## [1] "numeric"
    ## 
    ## $d1_creatinine_max
    ## [1] "numeric"
    ## 
    ## $d1_creatinine_min
    ## [1] "numeric"
    ## 
    ## $d1_glucose_max
    ## [1] "integer"
    ## 
    ## $d1_glucose_min
    ## [1] "integer"
    ## 
    ## $d1_hco3_max
    ## [1] "numeric"
    ## 
    ## $d1_hco3_min
    ## [1] "numeric"
    ## 
    ## $d1_hemaglobin_max
    ## [1] "numeric"
    ## 
    ## $d1_hemaglobin_min
    ## [1] "numeric"
    ## 
    ## $d1_hematocrit_max
    ## [1] "numeric"
    ## 
    ## $d1_hematocrit_min
    ## [1] "numeric"
    ## 
    ## $d1_inr_max
    ## [1] "numeric"
    ## 
    ## $d1_inr_min
    ## [1] "numeric"
    ## 
    ## $d1_lactate_max
    ## [1] "numeric"
    ## 
    ## $d1_lactate_min
    ## [1] "numeric"
    ## 
    ## $d1_platelets_max
    ## [1] "integer"
    ## 
    ## $d1_platelets_min
    ## [1] "numeric"
    ## 
    ## $d1_potassium_max
    ## [1] "numeric"
    ## 
    ## $d1_potassium_min
    ## [1] "numeric"
    ## 
    ## $d1_sodium_max
    ## [1] "numeric"
    ## 
    ## $d1_sodium_min
    ## [1] "numeric"
    ## 
    ## $d1_wbc_max
    ## [1] "numeric"
    ## 
    ## $d1_wbc_min
    ## [1] "numeric"
    ## 
    ## $h1_albumin_max
    ## [1] "numeric"
    ## 
    ## $h1_albumin_min
    ## [1] "numeric"
    ## 
    ## $h1_bilirubin_max
    ## [1] "numeric"
    ## 
    ## $h1_bilirubin_min
    ## [1] "numeric"
    ## 
    ## $h1_bun_max
    ## [1] "numeric"
    ## 
    ## $h1_bun_min
    ## [1] "numeric"
    ## 
    ## $h1_calcium_max
    ## [1] "numeric"
    ## 
    ## $h1_calcium_min
    ## [1] "numeric"
    ## 
    ## $h1_creatinine_max
    ## [1] "numeric"
    ## 
    ## $h1_creatinine_min
    ## [1] "numeric"
    ## 
    ## $h1_glucose_max
    ## [1] "numeric"
    ## 
    ## $h1_glucose_min
    ## [1] "integer"
    ## 
    ## $h1_hco3_max
    ## [1] "numeric"
    ## 
    ## $h1_hco3_min
    ## [1] "numeric"
    ## 
    ## $h1_hemaglobin_max
    ## [1] "numeric"
    ## 
    ## $h1_hemaglobin_min
    ## [1] "numeric"
    ## 
    ## $h1_hematocrit_max
    ## [1] "numeric"
    ## 
    ## $h1_hematocrit_min
    ## [1] "numeric"
    ## 
    ## $h1_inr_max
    ## [1] "numeric"
    ## 
    ## $h1_inr_min
    ## [1] "numeric"
    ## 
    ## $h1_lactate_max
    ## [1] "numeric"
    ## 
    ## $h1_lactate_min
    ## [1] "numeric"
    ## 
    ## $h1_platelets_max
    ## [1] "integer"
    ## 
    ## $h1_platelets_min
    ## [1] "integer"
    ## 
    ## $h1_potassium_max
    ## [1] "numeric"
    ## 
    ## $h1_potassium_min
    ## [1] "numeric"
    ## 
    ## $h1_sodium_max
    ## [1] "numeric"
    ## 
    ## $h1_sodium_min
    ## [1] "numeric"
    ## 
    ## $h1_wbc_max
    ## [1] "numeric"
    ## 
    ## $h1_wbc_min
    ## [1] "numeric"
    ## 
    ## $d1_arterial_pco2_max
    ## [1] "numeric"
    ## 
    ## $d1_arterial_pco2_min
    ## [1] "numeric"
    ## 
    ## $d1_arterial_ph_max
    ## [1] "numeric"
    ## 
    ## $d1_arterial_ph_min
    ## [1] "numeric"
    ## 
    ## $d1_arterial_po2_max
    ## [1] "numeric"
    ## 
    ## $d1_arterial_po2_min
    ## [1] "numeric"
    ## 
    ## $d1_pao2fio2ratio_max
    ## [1] "numeric"
    ## 
    ## $d1_pao2fio2ratio_min
    ## [1] "numeric"
    ## 
    ## $h1_arterial_pco2_max
    ## [1] "numeric"
    ## 
    ## $h1_arterial_pco2_min
    ## [1] "numeric"
    ## 
    ## $h1_arterial_ph_max
    ## [1] "numeric"
    ## 
    ## $h1_arterial_ph_min
    ## [1] "numeric"
    ## 
    ## $h1_arterial_po2_max
    ## [1] "numeric"
    ## 
    ## $h1_arterial_po2_min
    ## [1] "numeric"
    ## 
    ## $h1_pao2fio2ratio_max
    ## [1] "numeric"
    ## 
    ## $h1_pao2fio2ratio_min
    ## [1] "numeric"
    ## 
    ## $aids
    ## [1] "integer"
    ## 
    ## $cirrhosis
    ## [1] "integer"
    ## 
    ## $hepatic_failure
    ## [1] "integer"
    ## 
    ## $immunosuppression
    ## [1] "integer"
    ## 
    ## $leukemia
    ## [1] "integer"
    ## 
    ## $lymphoma
    ## [1] "integer"
    ## 
    ## $solid_tumor_with_metastasis
    ## [1] "integer"
    ## 
    ## $diabetes_mellitus
    ## [1] "integer"

``` r
class(col_classes(Diabetes))
```

    ## [1] "list"

``` r
#Not sure how to write a function to get the names of columns with a great 
#percentage of NAs. will work on this. 

check_per <- function(y){
  if((sum(is.na(y))/nrow(y)) > 0.7) {return (TRUE)} 
  return (FALSE)
}

remove_col <- function(df){
  mapply(df, function(x) paste(check_per(x)))
}
```
