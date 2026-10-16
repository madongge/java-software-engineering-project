# Precision Medicine & Pharmacogenomics Knowledge Base Matching System

[![Java 11](https://img.shields.io/badge/Java-11-orange.svg)](https://www.oracle.com/java/)
[![Servlet 4.0](https://img.shields.io/badge/Servlet-4.0-blue.svg)](https://javaee.github.io/servlet-spec/)
[![MySQL 8.0](https://img.shields.io/badge/MySQL-8.0-00758F.svg)](https://www.mysql.com/)
[![Maven](https://img.shields.io/badge/Build-Maven-C71A36.svg)](https://maven.apache.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

An enterprise-grade Java web platform and relational pharmacogenomics knowledge base engineered for clinical precision medicine, developed as a software engineering group project at **Zhejiang University (ZJU)**.

The system integrates genomic variant annotations (ANNOVAR) with clinical pharmacogenomics repositories (PharmGKB, CPIC, FDA drug labels), enabling automated matching between patient single nucleotide variants (SNVs)/indels and clinical dosing guidelines to assist personalized medication.

---

## 🌟 Key Features

- **Pharmacogenomics Knowledge Base**:
  - Implements relational data models for drugs, FDA/EMA clinical drug labels, and CPIC dosing guidelines parsed from PharmGKB datasets.
  - Dedicated CLI data migration pipeline (`PharmGKBImporter`) for automated ingestion and JSON-to-relational ETL processing.
- **Clinical Genomic Variant Matching Engine**:
  - Ingests and parses high-throughput **ANNOVAR** functional annotation files across exome/genome sequencing data.
  - Interrogates multi-database pathogenicity scoring metrics (including SIFT, PolyPhen-2, CADD, GERP++, REVEL, and gnomAD/ExAC allele frequencies).
  - Matches sample genotypes to actionable clinical drug response phenotypes and adverse drug reaction (ADR) risk profiles.
- **Architectural Design**:
  - Clean **MVC (Model-View-Controller)** pattern with custom lightweight front-controller dispatching (`DispatchServlet`).
  - Robust **DAO (Data Access Object)** abstraction layer coupled with thread-safe JDBC connection pooling and parameterized query protection against SQL injection.
  - Responsive medical dashboard built with Bootstrap, JSTL, and asynchronous AJAX queries.

---

## 🛠️ System Architecture & Database Schema

```mermaid
flowchart TD
    A["Patient VCF / ANNOVAR Annotations"] -->|Upload & Parse| B["Matching Controller"]
    C["PharmGKB / CPIC Raw Data"] -->|PharmGKBImporter ETL| D["Relational MySQL Database"]
    
    subgraph MySQL Database ["MySQL 8.0 Knowledge Base (biomed)"]
        D1["annovar (Variant Pathogenicity)"]
        D2["drug (Active Pharmacological Agents)"]
        D3["drug_label (FDA/EMA Clinical Labels)"]
        D4["dosing_guideline (CPIC Prescribing Guidelines)"]
        D5["sample (Patient Metadata)"]
    end
    
    B --> D1
    B --> D4
    D --> D2
    D --> D3
    D --> D4
    B --> E["Clinical Dosing & Risk Recommendation Engine"]
    E --> F["Web Dashboard (JSP / Bootstrap / JSTL)"]
