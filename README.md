
# Tableau Interactive Company Linkage Dashboard

This dashboard was created in Tableau to visually and interactively explore corporate linkages using graph-based layouts. The goal is to allow investigators, researchers, and analysts to explore how companies are connected based on shared communication identifiers such as email addresses and phone numbers.
The dashboard was created with Assocate_Reset_VRelease.xlsx

##  Overview

The dashboard visualizes **Base Companies** as central nodes and their respective **Associated Companies** as connected nodes using a tree graph layout. Linkage values (emails/phones) are used to reveal hidden patterns of association, and dynamic filters enable deeper exploration.

---

##  Features & Functionalities

### Tree Graph Visualization
- Built using **Tableau Tree Extension** to create a force-directed radial layout.
- The central node represents the **selected base company**.
- Edges represent linkages, and nodes connected to the center are **associated companies**.

### Linkage Method Color Coding
- **Blue**: Email
- **Orange**: Phone
- Each association line is color-coded based on the type of linkage method.

### Filters and Controls
- **Company Selector**: Dropdown list to choose the base company.
- **Linkage Value Filter**: Checkbox list to filter based on shared email or phone identifiers.
- **Hover Tooltip Extension**: Hover over any node to view details like:
  - Associated company
  - Linkage method
  - Contact value (email/phone)

### Tabular View
- Displays selected base company, associated companies, and linkage metadata side-by-side.
- Updates dynamically based on filters and selections.

---

## 🛠 How It Was Built

1. **Data Preparation**:
   - Cleaned and structured datasets were loaded into Tableau.
   - Each record mapped a base company → linkage value → associated company.

2. **Worksheets Created**:
   - Filter controls for Base Company and Linkage Value.
   - Conditional formatting to apply color for different linkage methods.
   - Summary table for base and associated company relationship.

3. **Tree Extension**:
   - The tree extension was added from Tableau Extension Gallery.
   - Configured to dynamically highlight and reposition nodes based on selection.
   - Hover enhancements added for usability.

4. **Dashboard Assembly**:
   - Combined worksheets, filters, and graph into a single layout.
   - Made interactive using filter actions and extension event listeners.


Visulization:
![image](https://github.com/user-attachments/assets/7e2994ba-4988-43cf-94e4-146f5b517ebe)

---

##  Example

In the provided screenshot, **Shanghai Ditao Chemical Technology Co., Ltd** is selected:
- 20+ associated companies are shown in the tree view.
- Shared email (`sales@laidiou.com`) and phone (`P/F: 8651281664689`) values are shown.
- Hovering on a node highlights the company and displays linkage metadata.

---

## Requirements

- Tableau Desktop (2021.4+ recommended)
- Internet connection to load the Tableau Tree extension
- Tree Extenssions

---

## Files

- `DAEN 690_tableau Final 1.twbx`: Packaged Tableau workbook
- `data_linkages.csv`: Cleaned dataset used to power the dashboard

---

## Contributors

- Team ISO Linkage Graphers  
- DAEN 690 Capstone – Spring 2025

---

## License

This dashboard is provided for academic and investigative use.  
MIT License © 2025 Team ISO Linkage Graphers
