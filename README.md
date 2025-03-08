# Grammar of DataFrames

A Python library built on top of pandas that empowers you to perform data analysis using a "grammar of" style. The core idea is to treat each DataFrame as a variable and to express operations—both within and across DataFrames—as transformations that yield new DataFrames. No hidden intermediates, just clear, functional-like data flows.

## Core Principle

Every operation transforms a DataFrame into another DataFrame. Just like in arithmetic (e.g., `a + b`), our operations are designed to be composable and transparent:

- **One-to-One:** Operations where the number of rows is maintained.
- **Reducing:** Operations where rows are reduced (e.g., aggregation or curve fitting).
- **Expanding:** Operations where you may add computed columns or merge information from other DataFrames.

## Data Format Conventions

### Tidy Format

- **One Measurement per Row:** Each DataFrame should represent a single measurement type with one column named `value` for the actual measurement.
- **Distinct Measurements:** Different properties (e.g., `mscarlett` vs. `4MU fluorescence`) are stored in separate DataFrames.
- **Repeated Measurements:** Repeated measures (e.g., time courses) for the same sample belong in the same DataFrame.

### Types of Data

- **Measurements:** Data corresponding to a specific sample (e.g., well, time point).
- **Derived Data:** Calculated data (averages across replicates, fitted curves, etc.) derived from multiple measurements.

## Operations

### Within a DataFrame

These operations typically reduce the number of rows (e.g., aggregation, background subtraction):

- **Background Subtraction:**
    1. Compute background values in a new DataFrame
    2. Merge with the original
    3. Subtract to obtain background-corrected values
- **Curve Fitting:**
    1. Group data
    2. Apply a fit function to each group
    3. Return a DataFrame with fewer rows (one per group)
- **Normalization:**
    1. Determine normalization factors
    2. Merge and divide to obtain normalized values
- **Statistical Testing:**
    1. Fetch comparison (e.g., negative control) values
    2. Merge, group, and apply t-tests
    3. Return a reduced DataFrame with test statistics

### Across DataFrames

Operations that combine data from two or more DataFrames:

- **One-to-One Merges:** When both DataFrames have the same number of rows and the operation returns a DataFrame of equal shape.
- **Fewer-to-More Joins:** When merging a DataFrame with fewer rows (e.g., calibration factors) with a larger one.

### Final DataFrame Shape Considerations

- **Consistency:** Ideally, the final DataFrame should mirror the shape of the original measurement DataFrame.
- **Validation:** Emit a warning if the resulting DataFrame has zero elements, as this indicates a possible error in the operation.
- **Merging & Grouping:** Provide clear APIs for defining masks, merging criteria, and grouping columns.
