# project1-sequence-analysis
# Mismatch Analysis of Sequencing Reads

This code checks sequencing reads against a reference sequence and finds mismatches. It figures out whether samples treated with a genotoxic agent have more mismatches than control samples, and at which positions.

## How to run

Open the notebook in Google Colab and run the cells in order, including the `calculate_statistics` cell. The "Final analysis" cell runs everything and prints the final results.

## Inputs and outputs

**Input:** a list of read dictionaries (`read_id`, `sample`, `group`, `sequence`) and a reference string of A/C/G/T.

**Output:** for each group, a table of valid and invalid reads, % of reads with a mismatch, mismatch events and three positions, and a table of substitution counts. Finally, one line per top position compares control and treatment (means, difference, Welch's t-test, p-value).

## Code design

- **Invalid reads are flagged, not removed.** `validate_read` returns a dictionary of errors, so bad reads don't ruin the system of codes. I kept them in the output so they can be counted per sample. 
- **Dictionaries for counts.** `summarise_samples` stores `position_counts` and `substitution_counts` as dictionaries, so it works for any read length and stores only observed values. Percentages are calculated after counting and set to `None` when a sample has no valid reads.
- **Calculation separated from printing.** `print_summary` produces a display, so the returned data can be reused.
- **Ties.** Top positions are sorted by count, then by earlier position, as the task requires.

## Testing

1. Reads that were lowercase, too short, too long, or contained "N" (for example "atgc", "ATN", "ATGCC") gave the expected results and error codes.
2. Empty input returned `[]` from `analyse_reads` and `{}` from `summarise_samples`.
3. A sample with no valid reads gave `mismatch_percentage = None`, and the original read dictionaries were unchanged afterwards.
