# HPC

*EC2*
- EC2 hpc7g.4xlarge | 16vCPU | 128GB RAM | 200 Gigabit di rete | 1.8059 | 12 istanze | Irlanda | 
- Breakeven point al 70% -> on demand -> altrimenti 16000
- TOTALE = on demand 70% -> == savings
- TOTALE = on demand 100% -> 16000 * 36 mesi = 576000 $
- EC2 savings plans -> 11000 al mese
- TOTALE = savings plan -> 11000 * 36 mesi = 400000 $

*FSx for Lustre*
- FSx for Lustre modalità Scratch | 1,2 TB per nodo | 200 MBps/TiB
- 2220 al mese per tutti e 12
- TOTALE = 2220 * 36 mesi = 80000 $

*Head Scheduler*
- c8gn.8xlarge | 20500 all upfront 3 anni | sempre acceso
- TOTALE = 20500 $

# PONDUS

*EC2*
m6i.4xlarge 16vCPU | 64 GB RAM | 2 istanze | pagamento upfront | Stockholm
- TOTALE = 17000 $ / 15000 €

*RDS*
- db.r5.2xlarge 8vCPU | 64 GB RAM | bucket storage gp3 1,2 TB | backup DB 2 TB | Stockholm
- TOTALE = 2100 * 36 mesi = 75000 $ / 66000 €




# TOTALE
TOTALE HPC = 400000 + 80000 + 20500 = 500500 $ -> 440000 €
TOTALE PONDUS = 17000 + 75000 = 92000 $ -> 81000 €

HPC (saving plans) | 500500 $    | 440000 €  |
SYSTEM INTEGRATION |  40000 $    |  35000 €  |
PONDUS             |  92000 $    |  81000 €  |



TOTALE             | 631.500 $    | 556.000 €  |
