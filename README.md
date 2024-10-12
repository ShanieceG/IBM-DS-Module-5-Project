# IBM-DS-Module-5-Project
IBM Data Science Module 5 Project Q4

url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/stock.html."
html_data_2 = requests.get(url).text
soup = BeautifulSoup(html_data_2, "html.parser")
gme_tables = soup.find_all('table')
for index, table in enumerate(gme_tables):
    if ("GameStop Quarterly Revenue" in str(table)):
        gme_table_index = index

gme_revenue = pd.DataFrame(columns=["Date", "Revenue"])

for row in gme_tables[gme_table_index].tbody.find_all("tr"):
    col = row.find_all("td")
    if (col !=[]):
        date = col[0].text
        revenue = col[1].text.replace("$", "").replace(",", "")
        new_row = pd.DataFrame({"Date": [date], "Revenue":[revenue]})
        gme_revenue = pd.concat([gme_revenue, new_row], ignore_index=True)
