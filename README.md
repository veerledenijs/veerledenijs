SAMPLES = ['SRR8480619']

rule fastqc:
	input:
  
		"/mnt/2022/2022MBI_10/Project/1.fastq data/srr8480619/srr8480619_1_test.fastqc"
    
output:

		"/mnt/2022/2022MBI_10/Project/2.fastqc files/test/srr8480619_1_test.fastq_fastqc.html"
	
  conda:
  
		"envs/fastqc.yaml"

log:

      out="logs/fastqc.{sample}.out",
      err="logs/fastqc.{sample}.err"
      
 shell:
 
 


