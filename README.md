[srr8480619_1_test.fastq _fastqc.zip](https://github.com/veerledenijs/veerledenijs/files/9020410/srr8480619_1_test.fastq._fastqc.zip)
[srr8480619_2_test.fastq _fastqc.zip](https://github.com/veerledenijs/veerledenijs/files/9020414/srr8480619_2_test.fastq._fastqc.zip)
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
 
 
    "fastqc {input} --outdir fastqc/ >{log.out} 2>{log.err}"
 

rule trimmomatic:
	input:
  
  
           "/mnt/2022/2022MBI_10/Project/2.fastqc files/test/srr8480619_1_test.fastq_fastqc.html"
	  
output:
	
          "/mnt/2022/2022MBI_10/Project/4.Trimmomatic/test/forward_paired.fq"
          "/mnt/2022/2022MBI_10/Project/4.Trimmomatic/test/forward_unpaired.fq"
          "/mnt/2022/2022MBI_10/Project/4.Trimmomatic/test/reverse_paired.fq"
          "/mnt/2022/2022MNI_10/Project/4.Trimmomatic/test/reverse_unpaired.fq"  
	  
conda:

         "envs/trimmomatic.yaml"
	 
log: 
          
	  out="logs/trimmomatic.{sample}.out",
           err="logs/trimmomatic.{sample}.err"

shell: 

           "trimmomatic PE "
		"{input.mnt/2022/2022MBI_10/Project/4.Trimmomatic/test/forward_paired.fq} {input.mnt/2022/2022MBI_10/Project/4.Trimmomatic/test/forward_unpaired.fq} "
		"{output.mnt/2022/2022MBI_10/Project/4.Trimmomatic/test/reverse_paired.fq} {output.mnt/2022/2022MNI_10/Project/4.Trimmomatic/test/reverse_unpaired.fq} "
		"{output.mnt/2022/2022MBI_10/Project/4.Trimmomatic/test/reverse_paired.fq} {output.mnt/2022/2022MNI_10/Project/4.Trimmomatic/test/reverse_unpaired.fq} "
		"SLIDINGWINDOW:4:28 MINLEN:25 "
		"ILLUMINACLIP:NexteraPE-PE.fa:2:40:15 >{log.out} 2>{log.err}"

rule bowtie2:
         input:
	 
	          "/mnt/studentfiles/2022/2022MBI_10/Project/4.Trimmomatic/test/forward_paired.fq"
                  "/mnt/studentfiles/2022/2022MBI_10/Project/4.Trimmomatic/test/reverse_paired.fq"

output:

                   "/mnt/studentfiles/2022/2022MBI_10/Project/6.Bowtie2/unaligned"
                   "/mnt/studentfiles/2022/2022MBI_10/Project/6.Bowtie2/aligned"
		   
conda:

                   "envs/bowtie2.yaml"
		   
shell:

                   "bowtie2 --un-conc "/mnt/studentfiles/2022/2022MBI_10/Project/6.Bowtie2/unaligned" -x C_quinquefasciatus -1 "/mnt/studentfiles/2022/2022MBI_10/Project/4.Trimmomatic/test/forward_paired.fq" -2 "/mnt/studentfiles/2022/2022MBI_10/Project/4.Trimmomatic/test/reverse_paired.fq" -S "/mnt/studentfiles/2022/2022MBI_10/Project/6.Bowtie2/aligned"

 rule trinity:
           input:
	   
	                	"/mnt/studentfiles/2022/2022MBI_10/Project/6.Bowtie2/unaligned.1"
                                "/mnt/studentfiles/2022/2022MBI_10/Project/6.Bowtie2/unaligned.2"
output:

                               	"/mnt/studentfiles/2022/2022MBI_10/Project/7.Trinity/trinity.Trinity.fasta"
                                "/mnt/studentfiles/2022/2022MBI_10/Project/7.Trinity/Trinity_stats.txt"
				
conda:

                               "envs/trinity.yaml"
			       
shell:

                              Trinity --seqType fq  --max_memory 20G --left "/mnt/studentfiles/2022/2022MBI_10/Project/6.Bowtie2/unaligned.1" --right  "/mnt/studentfiles/2022/2022MBI_10/Project/6.Bowtie2/unaligned.2"  --output "/mnt/studentfiles/2022/2022MBI_10/Project/7.Trinity/trinity" --full_cleanup perl "/mnt/studentfiles/2022/2022MBI_10/miniconda3/pkgs/trinity-2.1.1-6/opt/trinity-2.1.1/util/TrinityStats.pl" "/mnt/studentfiles/2022/2022MBI_10/Project/7.Trinity/trinity.Trinity.fasta" > "/mnt/studentfiles/2022/2022MBI_10/Project/7.Trinity/Trinity_stats.txt"

			
rule blast:
            input:
		       
	                 "/mnt/studentfiles/2022/2022MBI_10/Project/7.Trintiy/trinity.Trinity.fasta"
		     
output:

                     "/mnt/studentfiles/2022/2022MBI_10/Project/8.Blast/dc_0.95-out test.txt"
		    
conda:               

                     "envs/blastn.yaml"
		   
shell: 

                         blastn -query "/mnt/studentfiles/2022/2022MBI_10/Project/7.Trinity/trinity.Trinity.fasta" -db dc_0.95 -out test.txt"

                         blastn -query "/mnt/studentfiles/2022/2022MBI_10/Project/7.Trinity/trinity.Trinity.fasta" -db dc_0.95 -out test3.txt -outfmt 6" 

