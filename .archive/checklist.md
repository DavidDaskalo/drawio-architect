# Claude Code Pre-Work Setup Checklist

## Overview
Before Claude Code can begin building the DrawIO Architect skill, you need to prepare the environment and gather necessary resources.

## Setup Checklist

### 1. Environment Setup

#### A. Install DrawIO Desktop (Required)
- [ ] Download and install DrawIO Desktop from: https://github.com/jgraph/drawio-desktop/releases
- [ ] Verify it opens and you can create a new diagram
- **Why**: Need to test that generated XML files actually work

#### B. Create Workspace Directory
- [ ] Create the base directory structure:
```bash
mkdir -p ~/drawio-architect-project
cd ~/drawio-architect-project
```

#### C. Install Required Tools (if not already available)
- [ ] Python 3.8+ (for JSON validation and testing)
- [ ] Node.js (optional, for any JS utilities)
- [ ] A good text editor (VS Code recommended)

### 2. Resource Gathering

#### A. Get Official GCP Icons
- [ ] Visit: https://cloud.google.com/icons
- [ ] Download the GCP architecture icons package (SVG format)
- [ ] Save to: `~/drawio-architect-project/reference-icons/gcp/`
- **Why**: Need visual reference for accurate descriptions

#### B. Access DrawIO Shape Libraries
- [ ] Open DrawIO Desktop
- [ ] Click: More Shapes... (bottom left)
- [ ] Enable "GCP" library (search for "gcp")
- [ ] Take screenshots of available GCP shapes
- [ ] Save screenshots to: `~/drawio-architect-project/reference-icons/drawio-gcp-shapes/`
- **Why**: Need to know what shapes are available

#### C. Find DrawIO GCP Shape Documentation
- [ ] Search GitHub: https://github.com/jgraph/drawio
- [ ] Look for GCP shape definitions (usually in XML/JSON format)
- [ ] If found, save to: `~/drawio-architect-project/references/`
- **Why**: Speeds up finding shape codes

### 3. Test Files Preparation

#### A. Save Your Original Diagram
- [ ] Save your original GCP architecture diagram image
- [ ] Save to: `~/drawio-architect-project/test-cases/original-diagram.png`
- **Why**: Primary test case

#### B. Create Simple Test Diagram
- [ ] Open DrawIO Desktop
- [ ] Create a simple GCP diagram with 3-4 services:
  - Cloud Run
  - BigQuery
  - Cloud Storage
  - VPC-SC container
- [ ] Save as: `~/drawio-architect-project/test-cases/simple-test.drawio`
- [ ] Export as PNG: `~/drawio-architect-project/test-cases/simple-test.png`
- **Why**: Provides a known-good reference file to analyze

#### C. Document Shape Codes from Test Diagram
- [ ] Open `simple-test.drawio` in a text editor
- [ ] Find and document 3-5 shape codes manually
- [ ] Save notes to: `~/drawio-architect-project/references/known-shapes.md`
- **Example**:
```markdown
# Known GCP Shape Codes

## Cloud Run
- Shape code: `shape=mxgraph.gcp2.cloud_run`
- Style string: `sketch=0;html=1;...`

## BigQuery
- Shape code: `shape=mxgraph.gcp2.bigquery`
- Style string: `sketch=0;html=1;...`
```
- **Why**: Gives Claude Code starting examples to work from

### 4. Claude Code Configuration

#### A. Prepare the MVP Spec
- [ ] Save the MVP spec to: `~/drawio-architect-project/MVP-SPEC.md`
- [ ] Review it one final time for clarity

#### B. Create Initial Prompt for Claude Code
- [ ] Create file: `~/drawio-architect-project/INSTRUCTIONS.md`
- [ ] Contents:
```markdown
# Instructions for Claude Code

## Your Mission
Build the DrawIO Architect skill as specified in MVP-SPEC.md

## Start Here
1. Read MVP-SPEC.md completely
2. Review the resources in /reference-icons/ and /references/
3. Analyze test-cases/simple-test.drawio to understand DrawIO XML structure
4. Begin with Task 1: Building the GCP icons library

## Resources Available
- MVP-SPEC.md: Complete project specification
- test-cases/original-diagram.png: Primary test case (your diagram)
- test-cases/simple-test.drawio: Reference DrawIO file to analyze
- test-cases/simple-test.png: What the reference file looks like
- reference-icons/gcp/: Official GCP architecture icons
- reference-icons/drawio-gcp-shapes/: Screenshots of available DrawIO shapes
- references/known-shapes.md: Some pre-documented shape codes

## Output Directory
Create all skill files in: /mnt/skills/user/drawio-architect/

## Success Criteria
Your work is complete when you can recreate test-cases/original-diagram.png
as a valid DrawIO file with 85%+ accuracy.

## Questions?
If you need clarification on anything, document your questions in QUESTIONS.md
```

#### C. Set Working Directory
- [ ] Ensure Claude Code will start in: `~/drawio-architect-project`

### 5. Optional But Helpful

#### A. Create Examples Collection
- [ ] Find 2-3 more GCP architecture diagrams online
- [ ] Save to: `~/drawio-architect-project/inspiration/`
- **Why**: Helps understand common patterns

#### B. Research DrawIO XML Format
- [ ] Read: https://www.drawio.com/doc/faq/
- [ ] Find XML format documentation
- [ ] Save key findings to: `~/drawio-architect-project/references/drawio-xml-notes.md`
- **Why**: Helps Claude Code understand the format faster

#### C. Test DrawIO Shape Creation
- [ ] In DrawIO, manually create a Cloud Run service
- [ ] Save the file
- [ ] Open in text editor and examine the XML
- [ ] Document what you learn
- **Why**: Provides hands-on understanding

### 6. Final Verification

Run through this checklist:
- [ ] DrawIO Desktop is installed and working
- [ ] Original diagram is saved as test-cases/original-diagram.png
- [ ] Simple test diagram created (both .drawio and .png)
- [ ] GCP icons downloaded to reference-icons/gcp/
- [ ] DrawIO GCP shapes screenshot saved
- [ ] At least 3-5 shape codes manually documented
- [ ] MVP-SPEC.md is in project root
- [ ] INSTRUCTIONS.md is created for Claude Code
- [ ] Working directory is set up correctly

## Estimated Setup Time
- **Quick setup** (minimum): 30-45 minutes
- **Thorough setup** (recommended): 1-2 hours

## Directory Structure After Setup

```
~/drawio-architect-project/
├── MVP-SPEC.md                          # The full specification
├── INSTRUCTIONS.md                       # Start here instructions for Claude Code
├── QUESTIONS.md                          # (will be created by Claude Code if needed)
├── test-cases/
│   ├── original-diagram.png             # Your GCP diagram (primary test)
│   ├── simple-test.drawio               # Simple reference diagram
│   └── simple-test.png                  # Visual of simple diagram
├── reference-icons/
│   ├── gcp/                             # Official GCP architecture icons
│   └── drawio-gcp-shapes/               # Screenshots of DrawIO GCP library
├── references/
│   ├── known-shapes.md                  # Pre-documented shape codes
│   ├── drawio-xml-notes.md              # (optional) XML format notes
│   └── gcp-shape-definitions.json       # (if found) Official shape definitions
└── inspiration/                          # (optional) More example diagrams
```

## Ready to Begin?

Once all critical items are checked:
1. Open Claude Code in the project directory
2. Point it to INSTRUCTIONS.md
3. Let it read MVP-SPEC.md
4. Watch it work through the tasks!

## Troubleshooting Setup Issues

### Issue: Can't find DrawIO GCP shapes
**Solution**: In DrawIO, click "More Shapes" → Search "GCP" or "Google Cloud"

### Issue: Don't know how to extract shape codes
**Solution**: 
1. Create any shape in DrawIO
2. Save file
3. Open in text editor
4. Search for "mxgraph" to find shape codes

### Issue: Don't have the original diagram anymore
**Solution**: Use the diagram from this conversation or find a similar GCP architecture diagram online

### Issue: DrawIO Desktop won't install
**Solution**: Use web version at https://app.diagrams.net/ (works the same way)

---

## Next Steps After Setup

Once setup is complete:
1. Start Claude Code
2. Give it the project directory path
3. Point it to INSTRUCTIONS.md
4. Monitor progress through the 6 tasks
5. Test the output with your original diagram

**Expected Development Time**: 9-13 hours (per MVP spec)
**Your Setup Time**: 30 minutes - 2 hours
**Total Time to Working Skill**: ~10-15 hours