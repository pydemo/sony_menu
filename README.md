# Sony A7R V CAMSET File Generator

A Python tool for generating Sony A7R V camera settings (CAMSET) files with custom aspect ratios.

## Background

The Sony A7R V camera uses proprietary binary CAMSET files to store camera settings, including aspect ratio configurations. This tool enables users to generate custom aspect ratio settings files by analyzing and modifying the binary structure of existing reference files.

## Features

- Generate CAMSET files with custom aspect ratios (e.g., 1:1, 5:4, etc.)
- Uses reference files to understand and preserve the binary file structure
- Automatically updates timestamp information
- Intelligently maps custom aspect ratios based on patterns observed in existing files

## Installation

Clone this repository to your local machine:

```bash
git clone https://github.com/yourusername/sony-camset-generator.git
cd sony-camset-generator
```

No additional dependencies are required beyond Python 3.6+.

## Usage

```bash
python generate_camset.py <ratio_x> <ratio_y> [output_file]
```

### Arguments

- `ratio_x`: Numerator of the aspect ratio (e.g., 1 for 1:1, 5 for 5:4)
- `ratio_y`: Denominator of the aspect ratio (e.g., 1 for 1:1, 4 for 5:4)
- `output_file`: (Optional) Path for the output file. If not specified, the file will be saved as `CAMSET/custom_<x>x<y>.DAT`

### Examples

Generate a 1:1 (square) aspect ratio file:
```bash
python generate_camset.py 1 1
```

Generate a 5:4 aspect ratio file with a custom output path:
```bash
python generate_camset.py 5 4 my_custom_ratio.DAT
```

## How It Works

The tool:
1. Analyzes binary patterns in existing CAMSET files (3:2, 4:3, 16:9)
2. Identifies locations where aspect ratio data is encoded
3. Interpolates the correct byte values for custom ratios
4. Updates timestamps and other metadata
5. Writes the modified binary data to a new file

## Findings on the CAMSET File Format

Based on analysis of the binary files:

- Files start with a header containing "SONY0ILCE-7RM5" (camera model)
- Timestamp in format "YYYYMMDD_HHMMSS" located at offset 0x10-0x23
- Primary aspect ratio indicator at offset 0x24
- Known values:
  - 3:2: `00 CA 00 00`
  - 4:3: `00 B6 00 00`
  - 16:9: `00 6A 00 00`
- Aspect ratio encoding appears to use an inverse relationship with scaling

## Limitations

- The Sony CAMSET file format is proprietary and undocumented
- This tool uses reverse engineering to reproduce the file structure
- Using modified CAMSET files is at your own risk
- Not all possible aspect ratios have been tested

## License

This project is licensed under the MIT License - see the LICENSE file for details.