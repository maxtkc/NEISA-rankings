# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a NEISA (New England Intercollegiate Sailing Association) rankings calculator that generates rankings for both coed and women's sailing teams based on regatta results. The system fetches data from Google Sheets and processes sailing regatta results to calculate team rankings using a complex scoring algorithm.

## Core Architecture

The system is built around four main Python modules:

- **Runner.py**: Main execution script that orchestrates both coed and women's rankings calculations
- **rank_calculator.py**: Core ranking algorithm that implements the NEISA scoring system with School objects and regatta type handling
- **techscore_reader.py**: Web scraper that extracts regatta results from TechScore HTML tables
- **google_sheets.py**: Simple CSV reader for Google Sheets published as CSV

## Key Data Flow

1. Google Sheets contain school lists and regatta metadata (links and types)
2. TechScore links are scraped for actual regatta results
3. Results are processed through regatta-type-specific scoring algorithms
4. Schools accumulate points (top 4 regular regattas + 1 championship score)
5. Final rankings are output to CSV files

## Common Commands

### Running Rankings Calculation
```bash
python3 Runner.py
```
This generates four output files:
- `rankings.csv` - Coed team rankings
- `component_scores.csv` - Coed component score breakdown
- `womensrankings.csv` - Women's team rankings  
- `womens_component_scores.csv` - Women's component score breakdown

### Generating Scoring Tables
```bash
python3
>>> import rank_calculator
>>> rank_calculator.calculate_score_table(max_teams, min_teams, "regatta_type")
>>> exit()
```

Example: `rank_calculator.calculate_score_table(18, 16, "A")` prints scoring tables for A-type regattas with 18, 17, and 16 teams.

## Regatta Types and Scoring

The system handles multiple regatta types with different scoring scales:
- **A**: 8.5 points for first place (18 team minimum)
- **B**: 5.01 points for first place (16 team minimum)  
- **C**: 4.16 points for first place
- **SC/WSC**: Championship regattas (10.0 points, counted separately)
- **A-/WA/WB**: Variants with adjusted scoring

Championship scores (SC_A, SC_B, WSC_A) are handled separately from regular regatta points and each school can only have one championship score.

## Configuration

Update Google Sheets links in Runner.py:
- `schoolslink`: Published CSV of all NEISA schools
- `coedRegattaLink`: Published CSV of coed regatta metadata
- `womensRegattaLink`: Published CSV of women's regatta metadata

Links must be "Publish to Web" CSV URLs from Google Sheets, not sharing links.