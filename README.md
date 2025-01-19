# COVID-19 Data Exploration Project

## Overview
This project focuses on exploring global COVID-19 data from January 2020 to March 2021, covering one year of the pandemic's impact. The aim is to analyze the damage and trends a virus can cause within a year, providing valuable insights for understanding and preparing for similar situations, such as the rise of new viruses like HMPV. By leveraging SQL queries and techniques, the analysis uncovers trends and insights into COVID-19 cases, deaths, and vaccination progress across different countries and continents. The exploration demonstrates skills in data analysis and manipulation, serving as a showcase for advanced SQL capabilities.

---

## Skills and Techniques Used
- Joins
- Common Table Expressions (CTEs)
- Temporary Tables
- Window Functions
- Aggregate Functions
- Creating Views
- Data Type Conversion

---

## Data Sources
The project uses the following datasets:
1. **CovidDeaths.xlsx**: Contains data on COVID-related deaths.
2. **CovidVaccinations.xlsx**: Contains data on COVID vaccinations.

The data is queried and processed using SQL Server Management Studio (SSMS).

---

## Key Analyses and Insights

### 1. **Total Cases vs Total Deaths**
- Assesses the likelihood of dying if infected with COVID-19 in a specific country.
- Calculates the death percentage using:
  ```sql
  SELECT location, date, total_cases, total_deaths,
         ((total_deaths/total_cases)*100) AS Death_Percentage
  FROM PortfolioProject..CovidDeaths
  WHERE location LIKE '%india%'
  ORDER BY 1,2;
  ```

### 2. **Infection Rate Compared to Population**
- Determines the percentage of the population infected with COVID-19.
- Identifies countries with the highest infection rates relative to their population.

### 3. **Highest Death Counts**
- Highlights countries and continents with the highest death counts and death percentages.
- Example query for continents:
  ```sql
  SELECT continent, MAX(CAST(total_deaths AS INT)) AS TotalDeathCount
  FROM PortfolioProject..CovidDeaths
  WHERE continent IS NOT NULL
  GROUP BY continent
  ORDER BY TotalDeathCount DESC;
  ```

### 4. **Vaccination Progress**
- Tracks vaccination progress by calculating the percentage of the population that received at least one dose of the COVID vaccine.
- Uses CTEs and temporary tables to perform calculations.
  - Example of a CTE:
    ```sql
    WITH PopvsVac (Continent, Location, Date, Population, New_Vaccinations, RollingPeopleVaccinated)
    AS (
      SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations,
             SUM(CONVERT(INT, vac.new_vaccinations)) OVER(PARTITION BY dea.location ORDER BY dea.location, dea.date) AS RollingPeopleVaccinated
      FROM PortfolioProject..CovidDeaths dea
      JOIN PortfolioProject..CovidVaccinations vac
        ON dea.location = vac.location AND dea.date = vac.date
      WHERE dea.continent IS NOT NULL
    )
    SELECT *, (RollingPeopleVaccinated / Population) * 100 AS PercentPopulationVaccinated
    FROM PopvsVac;
    ```

### 5. **Global Metrics**
- Calculates worldwide totals for cases, deaths, and death percentages.
- Provides a snapshot of the pandemic’s overall impact.

---

## Project Highlights
- Focuses on analyzing one year of COVID-19 data to understand the immediate impact of a virus.
- Used advanced SQL techniques like window functions and CTEs to derive insights.
- Created views and temporary tables to structure data for easy visualization and analysis.
- Highlighted significant differences in COVID-19 impacts by region and country.

---

## Getting Started

### Prerequisites
- SQL Server Management Studio (SSMS)
- Datasets:
  - `CovidDeaths.xlsx`
  - `CovidVaccinations.xlsx`

### Steps
1. Clone this repository:
   ```bash
   git clone https://github.com/vsumitwork/Virus-Impact-Analysis.git
   ```
2. Open the SQL file (`COVID Portfolio Project - Data Exploration.sql`) in SSMS.
3. Load the `CovidDeaths.xlsx` and `CovidVaccinations.xlsx` datasets into your database.
4. Execute the SQL scripts to explore the data and generate insights.

---

## Technologies Used
- SQL Server Management Studio (SSMS)
- SQL for data exploration and manipulation

---

## License
This project is licensed under the MIT License. See the LICENSE file for more details.

---

## Acknowledgments
- COVID data sourced from publicly available datasets for educational purposes.
- This project was inspired by the global need to understand and visualize the impacts of the COVID-19 pandemic.
- Focused on analyzing the first year of the pandemic to showcase how quickly a virus can affect the world.
- Aims to provide valuable insights for understanding and preparing for emerging threats like the HMPV virus.
