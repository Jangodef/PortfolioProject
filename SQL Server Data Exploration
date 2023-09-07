
SELECT *
From PortfolioProject..Coviddeathsinfo
Where continent is not NUll
order by 3,4


--Data we are Selecting 

SELECT location,date,total_cases,new_cases,total_deaths,population
FROM dbo.CovidDeathsinfo
Where continent is not NUll
order by 1,2

--Looking Total cases vs total deaths \ From all the cases how many deaths. What's the porcentage of people who died ?
--Shows the likelihhod of dying if you contract covid in your country.
--Looking the Total cases vs Population
--Show the percentage of people that contract covid 

SELECT location,date,total_cases,new_cases,(total_deaths), (total_deaths/total_cases)* 100 AS Deathporcentage
FROM dbo.CovidDeathsinfo
WHERE location like '%Chile%'and
continent is not NUll
order by 1,2



SELECT location,date,population,total_cases,(total_cases/population)* 100 AS covid_contagous_percentage
FROM dbo.CovidDeathsinfo
WHERE location like '%states%' and continent is not NUll
order by 1,2

--Looking the countries with the Highest Infection Rate compared to Population

SELECT 
location,
population,
MAX(total_cases) AS HighestinfectionCount ,
MAX((total_cases/population))* 100 AS covid_contagious_percentage
FROM dbo.CovidDeathsinfo
Where continent is not NUll
GROUP BY location,population
order by covid_contagious_percentage desc



-- Countries with Highest Death Count per Population

SELECT 
location,
MAX(total_deaths) AS total_deaths_count
FROM dbo.CovidDeathsinfo
Where continent is not NUll
GROUP BY location
order by total_deaths_count desc

-- Continents with the highest deaths count

SELECT 
continent,MAX(total_deaths) AS total_deaths_count
FROM dbo.CovidDeathsinfo
Where continent is not NUll
GROUP BY continent
order by total_deaths_count desc

-- GLOBAL NUMBERS

SELECT date,
SUM(new_cases) as totalnewcases, 
sum(new_deaths) as totalnewdeaths,
sum(new_deaths)/SUM(new_cases) *100 global_percentage
FROM dbo.CovidDeathsinfo
WHERE continent is not NUll 
GROUP BY date
order by 1,2

-- Global numbers for the world.

SELECT 
SUM(new_cases) as total_newcases, 
sum(new_deaths) as total_newdeaths,
sum(new_deaths)/SUM(new_cases) *100 global_percentage
FROM dbo.CovidDeathsinfo
WHERE continent is not NUll 
order by 1,2


 /*JOIN TABLES
 -- Looking at total population vs vaccination 
 Calculating to add the value for new vaccinations each day plus the day before (Sum ),
 In a new column using OVER (PARTITION BY), which acts similar to the GROUP BY Clause, 
 but this one just isolates the information for the specific group that you intended to do an agregated function for
     */

SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations,
SUM(vac.new_vaccinations) OVER (Partition by dea.location order by dea.location,dea.date) AS RollingPeopleVaccinated
From PortfolioProject..CovidDeathsinfo dea
JOIN PortfolioProject..covidVaccinationsinfo vac
ON dea.location = vac.location
  AND dea.date = vac.date
  Where dea.continent is not null 
  Order by 2,3
  
  /* If i want to calculate the TOTAL POPULATION VS VACCINATIONS , using  (RollingPeopleVaccinated / population) *100 
   in the upper query it won't work. I have to create either a TEMP TABLE or CTE, so i can perform this calculation */

   --USE CTE

   With CTEpopvsvac (continent,location,date,population,new_vaccinations,RollingPeopleVaccinated)

   AS (

SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations,
SUM(vac.new_vaccinations) OVER (Partition by dea.location order by dea.location,dea.date) AS RollingPeopleVaccinated
From PortfolioProject..CovidDeathsinfo dea
JOIN PortfolioProject..covidVaccinationsinfo vac
ON dea.location = vac.location
  AND dea.date = vac.date
  Where dea.continent is not null 
  )
  SELECT *, (RollingPeopleVaccinated/population)*100 as PercentagePopvsVac
  From CTEpopvsvac
  

  -- TEMP TABLE

  DROP Table if exists #percentPopulationVaccinated

  CREATE Table #percentPopulationVaccinated
  (
  continent nvarchar(255),
  location nvarchar (255),
  date date,
  population numeric,
  new_vaccinations numeric,
  RollingPeopleVaccinated numeric
  )
INSERT INTO #percentPopulationVaccinated

SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations,
SUM(vac.new_vaccinations) OVER (Partition by dea.location order by dea.location,dea.date) AS RollingPeopleVaccinated
From PortfolioProject..CovidDeathsinfo dea
JOIN PortfolioProject..covidVaccinationsinfo vac
ON dea.location = vac.location
  AND dea.date = vac.date
  Where dea.continent is not null 
  

  SELECT *, (RollingPeopleVaccinated/population)*100 as PercentagePopvsVac
  From #percentPopulationVaccinated
  order by 2,3

  /* Creating views to store data for future visualizations */



  CREATE View MortalitybyContinent as

 SELECT 
continent,MAX(total_deaths) AS total_deaths_count
FROM dbo.CovidDeathsinfo
Where continent is not NUll
GROUP BY continent

 












