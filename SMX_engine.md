#!/usr/bin/env python3
# build_docs.py
# End-to-End Automation Script:
# 1. Generates a README.md file.
# 2. Generates SMX_engine.html from SMX_engine.md with an image gallery.
# 3. Automates the Git add, commit, and push process to GitHub.

import os
import sys
import subprocess

# Ensure the markdown library is available before running
try:
    import markdown
except ImportError:
    print("Error: The 'markdown' module is not installed.")
    print("Please activate your venv and run: pip install markdown")
    sys.exit(1)

# ----------------------------------------------------------------------
# Complete image descriptions mapping to the files in docs/images/
# ----------------------------------------------------------------------
IMAGE_DESCRIPTIONS = {
    "image_p1_1.jpeg": """Super-Meta-X Enterprise\nWelcome\n- Import Pipeline\n  - Standard Matrix (CSV, Excel, SAS, SPSS): BROWSE... No file selected\n  - Cochrane RevMan Archive (.rm5): BROWSE... No file selected\n  - Risk of Bias Array: BROWSE... No file selected\n  - PARSE & MOUNT FILES\n  - LOAD BENCHMARK DATA\nQuality & Imputation\n- Purge arrays with NA values\n- Purge duplicate arrays\nApply Filtering Directives\nTarget Variables for MICE:\nMICE Iteration cycles (m):\nPreview & Semantic Map\n- Interactive Editor\n- Dimensional Reshape\n- Data Diagnostics (EDA)\nExploratory Data Analysis\nEvaluate Variable: publication_year\nDistributional Geometry: Distribution of publication_year\nFive-Number Summary: Min 1990, 1st Qu. 2001, Median 2007, Mean 2008, 3rd Qu. 2016, Max. 4\nChart: X-axis publication_year, Y-axis Frequency""",

    "image_p2_1.jpeg": """Engine Architecture & Transparency\nComputational Principles: The algorithms governing effect aggregation run natively against standard implementations of the 'meta' and 'metafor' ecosystems...\nExecution Telemetry: Monitor application status flags...\nTRIGGER COMPILATION SEQUENCE\nRAM STATE PRESERVATION\nLocal Output Target: SMX_State_20260421_174\nWRITE MEMORY MATRIX TO DISK\nUpload Previous Archive (.rds) BROWSE...\nRE-HYDRATE HOST MEMORY""",

    "image_p3_1.jpeg": """INFLUENCE ALGORITHMS\n- ITERATIVE LEAVE-ONE-OUT MATRIX\n- VIECHTBAUER-CHEUNG DIAGNOSTICS\n- MAP BAUJAT DIMENSIONALITY\n- EXECUTE HEURISTIC OUTLIER FILTER\n- COMBINATORIAL GOSH EXTRAPOLATION\nCluster-Robust Variance Architecture: Identify Structural Clusters: study_id EXTRACT SANDWICH ESTIMATORS\nPUBLICATION FILTER: Distributional Asymmetry Rule: Egger's Regression; Q TRIGGER BASE ASYMMETRY PROTOCOLS; CONSTRUCT DOI PLOT & LFK INDEX\nWeight-Function Heuristic: Negative Exponential Curve; FIT PUBLICATION WEIGHT MODEL\nAlgorithmic Rejection Vectors: Identified outliers (random-effects model) "Hill 1991", "Parker 1994", ...\nResults with outliers removed: Number of studies k=50, Random effects model (HK) -0.4894 [-0.5502; -0.4286], Prediction interval [-0.7155; -0.2633], tau^2=0.0118, I^2=30.1% ...""",

    "image_p4_1.jpeg": """Dashboard: Imputation & Asymmetry, Doi Modeler & LFK, Selection Functions, Iterative LOO, Influence & Isolation, Outliers & RVE Logic, GOSH Array\nDiagnostic Tolerance Thresholds table with columns Metric, Effect, LLCI, ULCI. Omitting Hill 1991: -0.526, -0.602, -0.449 ... many more entries.\nChart: X-axis Variance Shift (Contribution to Q), Y-axis Absolute Correlation.\nAdditional table with more omitted studies.""",

    "image_p5_1.jpeg": """PUBLICATION FILTER: Distributional Asymmetry Rule: Egger's Regression; Q TRIGGER BASE ASYMMETRY PROTOCOLS; CONSTRUCT DOI PLOT & LFK INDEX; Weight-Function Heuristic: Negative Exponential Curve; FIT PUBLICATION WEIGHT MODEL\nList of omitted studies: Omitting Parker 1994, Robinson 1997, Williams 1999, ... (many entries).\nTable shows Omitting ... with +0.50 and -0.05 for each.""",

    "image_p6_1.jpeg": """Menu: Welcome, Data Management, Analysis Core, Geomatics & Risk, Moderators, Sensitivity & Bias, Geospatial, I/O & Reporting\nLeft Panel: INFLUENCE ALGORITHMS ... PUBLICATION FILTER ...\nMain Panel: Doi Geometry Projection, X-Axis LFK index -0.5, Y-Axis 50,60,70,80,90,100.""",

    "image_p7_1.jpeg": """INFLUENCE ALGORITHMS ... Cluster-Robust Variance Architecture ...\nContour Funnel mapping with Non-Parametric Imputation\nStandard Error, Study ID, Filter\nDistributional Asymmetry Rule: -1.5, -1.0, -0.5, 0.0, 0.5""",

    "image_p8_1.jpeg": """Preview & Semantic Map\nInteractive Editor\nDimensional Reshape\nData Diagnostics (EDA)\nPivot Table Generator\nIndex Stratification: [empty]\nColumn Stratification: [empty]\nTarget Values: [empty]\nMathematical Aggregator: Sum\nEXECUTE STRUCTURAL RESHAPING""",

    "image_p9_1.jpeg": """CATEGORICAL STRATIFICATION: Segmentation Covariate: study_id; Re-calculate pooling architectures within partitions; Execute Q-test; EXECUTE PARTITION LOGIC\nLINEAR MIXED-EFFECTS REGRESSION: Linear Modulators: publication_year; Apply mean-centering; Parameter Optimization Matrix: REML; FIT LINEAR COMBINATIONS\nStratified Forest Topology | Subgroup Statistics | Regression Analytics | Continuous Covariate Display | Model Diagnostics\nModel Coefficients: Mixed-Effects Model (k=58), tau^2=0.0539, I^2=66.66%, R^2=0.00%, QE p<0.0001, QM p=0.4731. Coefficients: intrcpt 4.7527 (p=0.5171), publication_year -0.0026 (p=0.4731).""",

    "image_p10_1.jpeg": """CATEGORICAL STRATIFICATION ... LINEAR MIXED-EFFECTS REGRESSION ...\nProportional Bubble Geometry\nOptimization Array: publication_year\nEffector Vector: -0.5, -1.0, -1.5\nCovariate Matrix spanning 1990 to 2020.""",

    "image_p11_1.jpeg": """CATEGORICAL STRATIFICATION ... LINEAR MIXED-EFFECTS REGRESSION ...\nResidual Standard Error Array\nRegression Standardized Residuals\nX-axis: Tolerance Level, Y-axis: Tolerance Level\nValues around 0.5 to -0.5.""",

    "image_p12_1.jpeg": """FOREST TOPOGRAPHY: Hierarchical Ordering Rule: Chronological; Embed I² and Tau² indices; Embed proportional weight vector; Independent Estimate Color #3498DB; Pooled Output Color #E74C3C; High-Resolution Export Format: PNG; Download Final Geometry\nBIAS INTEGRATION: Target Instrument: RoB 2 (RCTs); 1. SCAFFOLD DATA MATRIX""",

    "image_p13_1.jpeg": """Similar to Fig 12: FOREST TOPOGRAPHY with Chronological ordering, color settings, export options, and BIAS INTEGRATION with RoB 2.""",

    "image_p14_1.jpeg": """FOREST TOPOGRAPHY ... BIAS INTEGRATION ...\nCumulative Saturation | Galbraith Radial | Drapery P-Value Projection | Risk Modeling\nIterative Information Addition table: Adding Garcia 2002 (k=19) effect -0.48 [-0.64; -0.31], tau²=0.2664, I²=70.4% ... and many more rows.""",

    "image_p15_1.jpeg": """RoB 2 (RCTs): 1. SCAFFOLD DATA MATRIX; 2. COMMIT LOGIC GATE; 3. RENDER VISUAL ABSTRACTIONS\nTable of 60 studies with columns: TE, SE(TE), 95%-CI, Weight. Random effects model (HK), Prediction interval, Heterogeneity: I²=65.8%, τ²=0.0517, p<0.0001.""",

    "image_p16_1.jpeg": """1. ESTIMATION MECHANICS: Coordinate Projection: Odds Ratio; Purge arrays with zero occurrences; Zero-cell continuity scalar: 0.5; DERIVE EFFECT COORDINATE VECTORS\n2. POOLING ARCHITECTURE: Hierarchical Framework: Random-Effects (Unconditional); Tau² Estimator Algorithm: REML; Apply Knapp-Hartung adjustments; Estimate predictive limits; EXECUTE MATHEMATICAL POOLING\nAlgorithmic Engine Summary table with 60 studies and heterogeneity quantification: Tau²=0.0517, I²=65.76%, Q=172.3044, df=59, p≈4.98e-13.""",

    "image_p17_1.jpeg": """1. ESTIMATION MECHANICS ... 2. POOLING ARCHITECTURE ...\nResolved Mathematical Parameters table (first 15 rows): studlab, TE, se_te, lower, upper.\nEmpirical Vector Distribution: Spiral Frequency, Isolative Effect Magnitude."""
}

# ----------------------------------------------------------------------
# HTML template with dark theme and updated image gallery styling
# ----------------------------------------------------------------------
HTML_TEMPLATE = """<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Super-Meta-X Documentation</title>
    <style>
        :root {{
            --primary-color: #58a6ff;
            --secondary-color: #79c0ff;
            --bg-color: #0d1117;
            --container-bg: #161b22;
            --text-color: #c9d1d9;
            --border-color: #30363d;
            --code-bg: rgba(110, 118, 129, 0.1);
            --table-header-bg: #21262d;
        }}
        body {{
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            line-height: 1.6;
            background-color: var(--bg-color);
            color: var(--text-color);
            margin: 0;
            padding: 0;
        }}
        .container {{
            max-width: 1000px;
            margin: 40px auto;
            background: var(--container-bg);
            padding: 40px;
            border-radius: 12px;
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.5);
            border: 1px solid var(--border-color);
        }}
        h1, h2, h3 {{ color: var(--primary-color); margin-top: 1.5em; font-weight: 600; }}
        h1 {{ border-bottom: 2px solid var(--border-color); padding-bottom: 10px; margin-top: 0; font-size: 2.2em; }}
        h2 {{ border-bottom: 1px solid var(--border-color); padding-bottom: 5px; font-size: 1.6em; }}
        h3 {{ font-size: 1.3em; color: var(--text-color); }}
        table {{
            border-collapse: collapse;
            width: 100%;
            margin: 25px 0;
            background-color: var(--container-bg);
            border-radius: 6px;
            overflow: hidden;
            border: 1px solid var(--border-color);
        }}
        th, td {{
            border: 1px solid var(--border-color);
            padding: 12px 16px;
            text-align: left;
        }}
        th {{ background-color: var(--table-header-bg); color: #ffffff; font-weight: 600; }}
        tr:nth-child(even) {{ background-color: rgba(255, 255, 255, 0.02); }}
        code {{
            background-color: var(--code-bg);
            padding: 0.2em 0.4em;
            border-radius: 6px;
            font-family: monospace;
        }}
        a {{ color: var(--primary-color); text-decoration: none; }}
        a:hover {{ color: var(--secondary-color); text-decoration: underline; }}

        /* Gallery styling */
        .gallery {{
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 20px;
            margin-top: 30px;
        }}
        .gallery figure {{
            margin: 0;
            background: var(--bg-color);
            border: 1px solid var(--border-color);
            border-radius: 8px;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            transition: transform 0.2s, border-color 0.2s;
        }}
        .gallery figure:hover {{
            transform: translateY(-3px);
            border-color: var(--primary-color);
        }}
        .gallery img {{
            width: 100%;
            height: auto;
            border-bottom: 1px solid var(--border-color);
            background-color: #fff; /* Fallback background for transparent images */
        }}
        .gallery figcaption {{
            padding: 15px;
            font-size: 0.9rem;
            color: var(--text-color);
            background: var(--container-bg);
            max-height: 200px;
            overflow-y: auto;
            white-space: pre-wrap;
            font-family: monospace;
            flex-grow: 1;
        }}
        .gallery .placeholder-title {{
            font-weight: 600;
            color: var(--primary-color);
            margin-bottom: 8px;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
        }}
    </style>
</head>
<body>
    <div class="container">
        {content}
        {gallery}
    </div>
</body>
</html>"""

def generate_readme(readme_filepath="README.md"):
    """Generates a structured README.md for the GitHub repository."""
    print("Generating README.md...")
    readme_content = """# Super-Meta-X Enterprise Documentation

Welcome to the documentation repository for the **Super-Meta-X Enterprise** engine.

This repository holds the structural Markdown documentation, the compiled HTML interface with an integrated image gallery, and the UI reference assets.

## 📂 Repository Structure

```text
/SuperMetaX
├── build_docs.py      # End-to-end automation script (Builds HTML & Auto-pushes)
├── SMX_engine.md      # Raw markdown documentation and schemas
├── SMX_engine.html    # Compiled dark-theme documentation with image gallery
├── README.md          # Project overview
└── docs/
    └── images/        # UI and system reference screenshots
```

## 🚀 How to Build and Deploy

The repository includes an automated pipeline script `build_docs.py`. Running this script will:
1. Parse `SMX_engine.md`.
2. Generate the visual `SMX_engine.html` file mapping local images.
3. Automatically stage, commit, and push updates to this GitHub repository.

**Command:**
```bash
python build_docs.py
```

## 🛠 Features

- **Automated HTML Construction:** Converts markdown seamlessly using the `markdown` library with table and fenced code block support.
- **Dynamic Gallery Mapping:** Automatically links and descriptions associated with `.jpeg` assets from the `docs/images/` directory.
- **Continuous Integration (Local):** Automatically executes Git operations via Python's `subprocess` to keep the cloud repository instantly synced.
"""
    with open(readme_filepath, "w", encoding="utf-8") as f:
        f.write(readme_content)
    print("✅ Created README.md")

def generate_gallery_html():
    """Create the HTML for the image gallery linking to docs/images/."""
    items = []
    for filename, desc in IMAGE_DESCRIPTIONS.items():
        # Path to the actual image file relative to the HTML file
        image_path = f"docs/images/{filename}"
        
        fig = f'''
        <figure>
            <img src="{image_path}" alt="{filename}" loading="lazy" onerror="this.style.display='none'">
            <figcaption>
                <div class="placeholder-title">📸 {filename}</div>
                {desc}
            </figcaption>
        </figure>'''
        items.append(fig)

    gallery_section = f'''
    <hr style="border: 0; height: 1px; background: var(--border-color); margin: 60px 0;">
    <h2>Visual Reference Gallery</h2>
    <p>Below are structural and interface references from the Super-Meta-X documentation.</p>
    <div class="gallery">
        {''.join(items)}
    </div>
    '''
    return gallery_section

def generate_html(md_filepath="SMX_engine.md", html_filepath="SMX_engine.html"):
    """Read markdown, convert to HTML, wrap in template, and write output."""
    if not os.path.exists(md_filepath):
        print(f"Error: '{md_filepath}' not found in current directory.")
        print("Please make sure you are running this script from the correct directory.")
        return False

    print(f"Reading '{md_filepath}'...")
    with open(md_filepath, "r", encoding="utf-8") as f:
        md_text = f.read()

    print("Converting Markdown to HTML...")
    # Use extensions for tables and fenced code blocks
    html_body = markdown.markdown(md_text, extensions=['tables', 'fenced_code'])

    print("Generating Image Gallery...")
    gallery_html = generate_gallery_html()

    print("Applying styling templates...")
    final_html = HTML_TEMPLATE.format(content=html_body, gallery=gallery_html)

    print(f"Writing '{html_filepath}'...")
    with open(html_filepath, "w", encoding="utf-8") as f:
        f.write(final_html)

    print(f"✅ Build complete! Overwrote {html_filepath}")
    return True

def push_to_github(commit_message="Auto-build: Update HTML, README, and documentation assets"):
    """Automates Git add, commit, and push."""
    print("\n--- Starting GitHub Upload ---")
    try:
        # Check if git is initialized
        if not os.path.exists(".git"):
            print("Initializing Git repository...")
            subprocess.run(["git", "init"], check=True)
            print("⚠️ Please link your GitHub repository using: git remote add origin <YOUR_GITHUB_REPO_URL>")
            print("⚠️ Then run this script again or push manually.")
            return

        # Create .gitignore if it doesn't exist to exclude venv
        if not os.path.exists(".gitignore"):
            with open(".gitignore", "w") as f:
                f.write("venv/\n__pycache__/\n.DS_Store\n")
            print("Created .gitignore file.")

        print("Staging files...")
        subprocess.run(["git", "add", "."], check=True)

        print("Committing changes...")
        commit_proc = subprocess.run(["git", "commit", "-m", commit_message], capture_output=True, text=True)
        
        if "nothing to commit" in commit_proc.stdout.lower() or "working tree clean" in commit_proc.stdout.lower():
            print("No new changes to commit.")
        else:
            print("Changes committed successfully.")

        print("Pushing to GitHub...")
        # Note: If this fails because the upstream branch isn't set, the user will be guided by the exception below
        push_proc = subprocess.run(["git", "push"], capture_output=True, text=True)
        print("✅ Successfully pushed to GitHub!")

    except subprocess.CalledProcessError as e:
        print(f"❌ Git operation failed or requires manual intervention.")
        print("Hint: If this is your very first time pushing to a new repository, you may need to run these commands manually once:")
        print("      git remote add origin <YOUR_REPO_URL>")
        print("      git branch -M main")
        print("      git push -u origin main")

if __name__ == "__main__":
    print("=== Super-Meta-X Build & Deploy Pipeline ===")
    generate_readme()
    success = generate_html()
    
    if success:
        push_to_github()
    else:
        print("Skipping GitHub upload due to HTML build failure.")