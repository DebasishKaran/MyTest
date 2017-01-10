1)For Issue #1: SNP name are list of SNPs,
( e.g. rs35522752; rs367784979;rs369602394;rs372095550;rs372300786 etc.)

Solution: There is a DB constraint to allow only 50 characters for RS_IDs.
Comparing the chromosome location, usually ~2 rsids among the list in that position. In this case, rs35522752,rs367784979. 
Current solution: we chose the RS with the lowest number that matches the chromosome location: rs35522752

2)For Issue #2: Duplicated RS_IDs with different chromosome location like 
rs201551115     1     13414145     A     C     
rs201551115     1     13634942     A     C 

Solution : Keep the same rs_id that has the correct location, update the rs_id with rsid_chr:pos_a1/a2 like
rs201551115     1     13414145     A     C    — rs201551115_1:13414145_A/C
rs201551115     1     13634942     A     C    — rs201551115

3) For Issue #3: Duplicated SNPs with only QUAL difference like
SNP     CHR     BP      ALLELE1 ALLELE0 A1FREQ  BETA    SE      P       HWE_MLOGP       QUAL
.       10      102381257       G       GGCCCCC 0.998459        -0.015068       0.0549588       7.8E-01 0       0.84079 
.       10      102381257       G       GGCCCCC 0.998459        -0.015068       0.0549588       7.8E-01 0       0.77915

Solution: Choose the one with higher QUAL scores like
SNP     CHR     BP      ALLELE1 ALLELE0 A1FREQ  BETA    SE      P       HWE_MLOGP       QUAL
.       10      102381257       G       GGCCCCC 0.998459        -0.015068       0.0549588       7.8E-01 0       0.84079 –keep
.       10      102381257       G       GGCCCCC 0.998459        -0.015068       0.0549588       7.8E-01 0       0.77915 --remove

4) For Issue #4:Duplicated SNPs with the same QUAL scores and different pvalue like
SNP     CHR     BP      ALLELE1 ALLELE0 A1FREQ  BETA    SE      P       HWE_MLOGP       QUAL
.      10     109493917     T      TC     0.358462      -0.000817376  0.00412154    8.4E-01       0.011005       0.99592
.      10     109493917     T      TC     0.359296      -0.000544207  0.0041156     8.9E-01       0.011005       0.99592

Solution:Choose the one with lower pvalue. like
SNP     CHR     BP      ALLELE1 ALLELE0 A1FREQ  BETA    SE      P       HWE_MLOGP       QUAL
.      10     109493917     T      TC     0.358462      -0.000817376  0.00412154    8.4E-01       0.011005       0.99592—consider
.      10     109493917     T      TC     0.359296      -0.000544207  0.0041156     8.9E-01       0.011005       0.99592—remove

5)For Issue #5:Duplicated SNPs with the same pvalue, QUAL score and different Beta, EAF, SE
SNP     CHR     BP      ALLELE1 ALLELE0 A1FREQ  BETA    SE      P       HWE_MLOGP       QUAL
.       10      10766924        AG      A       0.484761        -0.00416359     0.00395524      2.9E-01 0.10941 0.99435  
.       10      10766924        AG      A       0.485757        -0.00415827     0.00394444      2.9E-01 0.10941 0.99435 

Solution:Choose the one with higher absolute beta values. Like
SNP     CHR     BP      ALLELE1 ALLELE0 A1FREQ  BETA    SE      P       HWE_MLOGP       QUAL
.       10      10766924        AG      A       0.484761        -0.00416359     0.00395524      2.9E-01 0.10941 0.99435   --consider
.       10      10766924        AG      A       0.485757        -0.00415827     0.00394444      2.9E-01 0.10941 0.99435    --remove

6) For Issue:Length of ALLELE0 and ALLELE1  were greater than 30.

Solution: There is a DB constraint to allow only 30 characters for EFFECT_ALLELE and OTHER_ALLELE.
So,30 characters has taken and truncated for  ALLELE0 and ALLELE1 .


