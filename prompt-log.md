# Prompt Log

## Stage 0 — Bio and Resume

### Prompt 1
Help me draft a 150–200 word professional bio for my GitHub profile README. I am a Business Management student at the University of Hawaii at Manoa with interests in finance, marketing, international business, and entrepreneurship. Make it specific and professional for recruiters and hiring managers.

### Prompt 2
Convert my current resume into Markdown format for a GitHub file named RESUME.md while keeping my education, work experience, leadership, skills, and awards accurate.

### Edits I Made
I reviewed and edited the AI-generated content to make sure it accurately reflected my experience, background, and professional interests.

## Stage 2 — Model Specification

### Prompt 1
Help me create a model specification for my FX hedging project using my assigned scenario. My firm is a U.S. technology services firm expecting a €12.5 million receivable. Include the required named ranges, workbook tabs, assumptions, calculation flow, sensitivity analysis, validation checks, and outputs.

### AI Draft and Correction
The initial AI draft assumed a 90-day settlement period even though my assigned scenario did not provide an exact settlement date. I corrected the specification by identifying the settlement timing as an indicative placeholder that must be confirmed and replaced with the correct information before using live market data.

### Edits I Made
I reviewed the specification to make sure it matched my assigned tech-services scenario and the Stage 2 requirements. I also made sure that market values not provided in my scenario were clearly identified as indicative placeholders rather than confirmed values.

## Stage 3 — Model Build and Audit

### Prompt 1
Using my Stage 2 specification, help me build the Excel workbook for my FX hedging scenario. The model should compare the forward, money-market, put option, and call option strategies and include the required inputs, formulas, sensitivity analysis, chart, and validation checks.

### Prompt 2
Help me review the completed workbook against the Stage 3 requirements and identify at least three findings for my build audit.

### Review and Edits
I reviewed the workbook after it was built and checked the inputs, hedge calculations, sensitivity analysis, and validation checks. I noticed that several market inputs are still placeholders and that the parity check does not currently pass because those inputs have not been updated with sourced market data. I left those values clearly identified for Stage 4 instead of changing them just to make the validation check pass. I also edited some of the workbook wording to make the notes clearer and more consistent with my project.