# Fine_Tuning_GPT2

## Cleaning the Short Stories File
<img width="646" alt="Screenshot 2025-01-09 at 10 16 30 PM" src="https://github.com/user-attachments/assets/bb5e333e-d1b7-4e27-be72-f27f5dac982c" />


Information about the text file:
1. Have to clean promotions given before the EOS
2. Already has delimiters like SOS, LN, and EOS

Steps taken to clean the file 
1. Defined unwanted patterns and removed them :
unwanted_pattern = r'<nl>.*?(\[.*?\]\(http[s]?://\S+\)|subscribe|/r/|PART \d+).*?<eos>'
