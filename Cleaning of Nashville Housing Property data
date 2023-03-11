/*

Cleaning Data in Nashville SQL Project

*/
------------------------------------------------------
----------------------Nashville SalesDate data cleaning

Select * --Selects everything within the NashvilleHousing table
from SQLProject.dbo.NashvilleHousing


-- Standardize Date Format for SalesDate

ALTER Table NashvilleHousing   -- Add column SaleDateConverted
Add SaleDateConverted Date;    -- Sets data type as date

-- Update table NashvilleHousing with a renamed column called SalesDateConverted
Update NashvilleHousing
SET SaleDateConverted = CONVERT(Date, SaleDate)

Select SaleDateConverted, CONVERT(Date, SaleDate)
from SQLProject.dbo.NashvilleHousing 

Update NashvilleHousing
SET SaleDate = SaleDate, CONVERT(Date, SaleDate)

-------------------------------------------------------------------

------------Fill input for Property Address data

Select *
from SQLProject.dbo.NashvilleHousing
--Where PropertyAddress is null
Order by ParcelID

-- Joining two tables to fill input Property Address column values where PropertyAddress column has null values
Select a.ParcelID, a.PropertyAddress, b.ParcelID, b.PropertyAddress, ISNULL(a.PropertyAddress,b.PropertyAddress)
from SQLProject.dbo.NashvilleHousing a -- created alias for Nashville table as a
JOIN SQLProject.dbo.NashvilleHousing b -- created alisa for Nashville table as b
	on a.ParcelID = b.ParcelID
	AND a.UniqueID <> b.UniqueID --establishing that the uniqueID column is not affected when code is executed
Where a.PropertyAddress is null

-- Updates table a with existing table b PropertyAddress column values
Update a
SET PropertyAddress = ISNULL(a.PropertyAddress,b.PropertyAddress)
from SQLProject.dbo.NashvilleHousing a -- created alias for Nashville table as a
JOIN SQLProject.dbo.NashvilleHousing b -- created alisa for Nashville table as b
	on a.ParcelID = b.ParcelID
	AND a.UniqueID <> b.UniqueID --establishing that the uniqueID column is not affected when code is executed
Where a.PropertyAddress is null

-----------------------------------------------------------------------
--Separating PropertyAddress into Individual columns. Such as: (Address, City, State)

Select PropertyAddress
from SQLProject.dbo.NashvilleHousing

Select
-- Column PropertyAddress is renamed to Address
-- Substring function searches for the character or value(,) and removes it
Substring(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) -1) as Address

--Substring adds the character or value),) at the very end of PropertyAddress value length
, Substring(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1 , LEN(PropertyAddress)) as Address 

From SQLProject.dbo.NashvilleHousing

--Creating two new columns: PropertySplitAddress, PropertySplitCity

ALTER Table NashvilleHousing
Add ProertySplitAddress Nvarchar(255);

-- Update table NashvilleHousing with a renamed column called SalesDateConverted
Update NashvilleHousing
SET ProertySplitAddress = Substring(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) -1)

ALTER Table NashvilleHousing
Add PropertySplitCity Nvarchar(255);

-- Update table NashvilleHousing with a renamed column called SalesDateConverted
Update NashvilleHousing
SET PropertySplitCity = Substring(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1 , LEN(PropertyAddress))

-- Run this code to view the updated table with the two newly added columns. PropertySplitAddress, PropertySplitCity
Select *
From SQLProject.dbo.NashvilleHousing

--Viewing data within the OwnerAddress column
Select OwnerAddress
From SQLProject.dbo.NashvilleHousing

-- Alternative method for seperating values within a given column into newly created seperate columns. Utilizing PARSENAME function
Select
PARSENAME(REPLACE(OwnerAddress, ',', '.') ,3)
,PARSENAME(REPLACE(OwnerAddress, ',', '.') ,2)
,PARSENAME(REPLACE(OwnerAddress, ',', '.') ,1)
From SQLProject.dbo.NashvilleHousing


ALTER TABLE NashvilleHousing   -- Adds a OwnerSplitAddress column
Add OwnerSplitAddress Nvarchar(255);

Update NashvilleHousing
SET OwnerSplitAddress = PARSENAME(REPLACE(OwnerAddress, ',', '.') ,3)

ALTER TABLE NashvilleHousing    -- Adds a OwnerSplitCity column
Add OwnerSplitCity Nvarchar(255);

Update NashvilleHousing
SET OwnerSplitCity = PARSENAME(REPLACE(OwnerAddress, ',', '.') ,2)

ALTER TABLE NashvilleHousing    -- Adds a OwnerSplitState column
Add OwnerSplitState Nvarchar(255);

Update NashvilleHousing
SET OwnerSplitState = PARSENAME(REPLACE(OwnerAddress, ',', '.') ,1)

-----------------------------------------------------------------------

-- Case Statements and Altering NashvilleHousing Table

-- Counts all distinct values within the SoldAsVacant column where 0 and 1 appear
Select Distinct(SoldAsVacant), Count(SoldAsVacant)
From SQLProject.dbo.NashvilleHousing
Group by SoldAsVacant -- 0 and 1 values within SoldAsVacant column are seperated into different rows
Order by SoldAsVacant -- 0 and 1 values are ordered into ascending order while remaining in seperate rows

--Utilizing Case statements / conditional formatting for the SoldAsVacant column
Select SoldAsVacant,
CASE
WHEN SoldAsVacant = 0 THEN 'No'
WHEN SoldAsVacant = 1 THEN 'Yes'
ELSE NULL
END AS UpdatedSoldAsVacant           --Stores 0 and 1 values and converts them into a new column with yes and no values
From SQLProject.dbo.NashvilleHousing

ALTER TABLE NashvilleHousing            -- Alter Nashville table to create a UpdatedSoldAsVacant column with a varchar data type
Add UpdatedSoldAsVacant varchar(255);   -- Changed new column default data type from bit to varchar 

Update NashvilleHousing SET             -- Copies all values from SoldAsVacant column to UpdatedSoldAsVacant column
	UpdatedSoldAsVacant = SoldAsVacant

Update NashvilleHousing               -- Replace 1 values with Yes values in UpdatedSoldAsVacant column
Set UpdatedSoldAsVacant = 'Yes'
Where UpdatedSoldAsVacant = '1'

Update NashvilleHousing               -- Replace 0 values with No values in UpdatedSoldAsVacant column
Set UpdatedSoldAsVacant = 'No'
Where UpdatedSoldAsVacant = '0'

-- Counts all distinct values within UpdatedSoldAsVacant column into a new column where Yes and No appear
Select Distinct(UpdatedSoldAsVacant), Count(UpdatedSoldAsVacant)
From SQLProject.dbo.NashvilleHousing
Group by UpdatedSoldAsVacant -- Yes and No values within UpdatedSoldAsVacant column are seperated into different rows
Order by UpdatedSoldAsVacant -- Yes and No values are ordered into ascending order while remaining in seperate rows

------------------------------------------------------------

-----Removing duplicate columns
--Utilize temp tables for removing duplicates for when it comes to larger databases (more efficient method)

WITH RowNumCTE AS(
Select *,
	ROW_NUMBER() OVER (
	PARTITION BY ParcelID,         --Partition By function is best used with unique information within a database
	             PropertyAddress,  --Unique information in the Nashville table is:
				 SalePrice,		   --ParcelID, PropertyAddress, SalesPrice, SaleDate, LegalReference
				 SaleDate,
				 LegalReference
				 Order BY 
				    UniqueID
					) row_num

From SQLProject.dbo.NashvilleHousing
)
Select *     -- Use Select command and code above to call on the CTE function known as RowNumCTE. Temp table creation
From RowNumCTE
Where row_num > 1
Order By PropertyAddress

---------------------------------------------------------------------------

---- Delete Unused Columns (handy for when temp columns are created). 
---- Raw data columns are never deleted. Industry standard best practice guidelines is to talk with someone who gives you the okay to delete raw data. 

Select *	--selects everything within the NashvilleHousing table							
From SQLProject.dbo.NashvilleHousing

Alter TABLE SQLProject.dbo.NashvilleHousing   --Alters table by deleting the following columns:  OwnerAddress, TaxDistrict, PropertyAddress
DROP COLUMN OwnerAddress, TaxDistrict, PropertyAddress

ALTER TABLE SQLProject.dbo.NashvilleHousing	  -- Alters Table by droping / deleting the SalesDate column
DROP COLUMN SaleDate
