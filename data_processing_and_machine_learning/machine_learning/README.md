# Demo for machine learning


#### Here is a demo to call peak and machine learning. We extract the reads on chr 3 for simplification. The code takes in the results of bed file generated by scCAT-seq data and wig file generated by Smart-seq2 data, spits out a csv file contains 8 features and predicttion:



* Features:
1) TPM_of_peak: The total Tags per Million(TPM) value of the peak called by CAGEr.
2) TPM_of_Dominant_Site: The highest TPM value of all sites within a peak.
3) Gene_length: The length of the transcript annotated.
4) Peak_width: The width of the peak called by CAGEr.
5) Dominant_TPM_to_Smart2: The ratio of Dominant_TPM to the RPM value of the corresponding gene revealed by Smart-seq2.
6) Slope_smart2_curve: The slope of Smart-seq2 coverage curve around the peaks.
7) Trend_of_smart2_reads: Calculated by dividing the number of reads increased/decreased within 50bp distance by 50.
8) Percentage: The percentage of read counts of a peak to the total counts of a transcript.

* Prediction:  
1) Predicted each individual peaks in the input file. The value "1" indicates a true TSS/TES peak while value "0" indicates a false TSS/TES peak

---
#### First, we run:

```
bash CAGE_dominant.sh 
```

* "D44_52_3tail_dominant_tes.bed" and "D44_52_5cap_dominant_tss.bed" file will be generated. These file is peak calling output.



#### Second, we run:

```
Rscript cal_slope_cattss.R D44_52_5cap_dominant_tss_upstream2k_and_genebody.bed D_merge_standard_smart_seq2.count
Rscript cal_slope_cattes.R D44_52_3tail_dominant_tes_genebody_downstream2k.bed D_merge_standard_smart_seq2.count
```

* "tc_D44_52_5cap_peak.csv" and "tc_D44_52_3tail_peak.csv" will be generated. In this sept, we added some features for machine learning.



#### Third, we run:

```
Rscript cal_slope_cattss2.R tc_D44_52_5cap_peak.csv D_merge_standard_smart_seq2_sorted_chr3.wig
Rscript cal_slope_cattes2.R tc_D44_52_3tail_peak.csv  D_merge_standard_smart_seq2_sorted_chr3.wig
```

* "tc_D44_52_5cap_peak.csvnew.csv" and "tc_D44_52_3tail_peak.csvnew.csv" will be generated. In this sept, we calculate the slope of Smart-seq2 coverage curve around the peaks.



#### Fourth, we run:

```
Rscript cal_slope_cattss3.R tc_D44_52_5cap_peak.csvnew.csv
Rscript cal_slope_cattes3.R tc_D44_52_3tail_peak.csvnew.csv
```

* "tc_D44_52_5cap_peak.csvnew.csv.csv" and "tc_D44_52_3tail_peak.csvnew.csv.csv" will be generated. In this sept, features were added and we can define the TRUE or FALSE of each peak.





#### Fifth, we run:

```
mv tc_D44_52_5cap_peak.csvnew.csv.csv ../data/
mv tc_D44_52_3tail_peak.csvnew.csv.csv ../data/
cd ../
python ./bin/demo.py
```

* The new folder named "output" will be built after completion of the pipeline, which contains all the output files, including the "\*.csv" file. 

* The last column in the .CSV files shows the predicted classification corresponding to each individual peaks in the input file. The value "1" indicates a true TSS/TES peak while value "0" indicates a false TSS/TES peak.


* from sept 1 to sept 5, it takes about 10 minutes.







