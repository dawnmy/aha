---
title: "Prepare the sequence file for making customized Kraken DB"
date: 2017-08-06T22:41:57+02:00
draft: false
---

```python
from Bio import SeqIO

# rename sequences with GI number
def rename_header(seq_file, gi_list_file, out_seq_file):
	fh = open(gi_list_file, "r")
	gi_list = [gi.strip() for gi in fh]

	fh.close()
	n = 0
	out_seqs = []
	for seq_record in SeqIO.parse(seq_file, "fasta"):
		if seq_record.seq:
			gi_header = "gi|{}|gb|{}".format(gi_list[n], seq_record.description)
			# print(gi_header)
			seq_record.id = gi_header #"gi|" + gi_list[n]
			seq_record.description = ""

			out_seqs.append(seq_record)
		n += 1
				# print(seq_record)
	# print(seq_record)
	SeqIO.write(out_seqs, out_seq_file, "fasta")


# retrive the representative 
def extract_seq(seq_file, name_list, out_seq_file):
	out_seqs = []
	for seq_record in SeqIO.parse(seq_file, "fasta"):
		for genome in name_list:
			if genome in seq_record.description:			
				out_seqs.append(seq_record)
				# print(seq_record)
	# print(seq_record)
	SeqIO.write(out_seqs, out_seq_file, "fasta")


if __name__ == '__main__':
	# rename_header("gv_all_refseq.fasta", "gv_all_refseq.gi", "gv_all_refseq_withgi.fa")
	#name_list = ["409-05", "6420B", "00703Dmash", "00703Bmash", "55152", "JCP8481B", "GED7760B", "HMP9231", "ATCC 14019", "23-12", "18-4"]
	name_list = ["Gardnerella vaginalis"]
	
	extract_seq("cpn60_ref_nut_seq.txt", name_list, "gv_cpn60_ref_nut_seq.fa")
	```

