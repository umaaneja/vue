from bs4 import BeautifulSoup

def merge_empty_leading_tds(html_content):
    soup = BeautifulSoup(html_content, 'html.parser')
    
    for table in soup.find_all('table'):
        rows = table.find_all('tr')
        if not rows:
            continue
            
        first_row = rows[0]
        tds = first_row.find_all('td')
        if not tds:
            continue
            
        # Find first non-empty td
        first_non_empty = None
        for i, td in enumerate(tds):
            if td.get_text(strip=True):
                first_non_empty = i
                break
                
        if first_non_empty is None or first_non_empty == 0:
            continue  # No empty tds to merge or first td already has content
            
        # Get content from first non-empty td
        content = tds[first_non_empty].decode_contents()
        
        # Create new td with colspan
        new_td = soup.new_tag('td', colspan=str(first_non_empty + 1))
        new_td.append(BeautifulSoup(content, 'html.parser'))
        
        # Replace all tds up to first non-empty with our new td
        for td in tds[:first_non_empty + 1]:
            td.decompose()
            
        first_row.insert(0, new_td)
                
    return str(soup)

# Example usage
html = """
<table>
  <tr>
    <td></td>
    <td></td>
    <td>Actual Content</td>
  </tr>
</table>
"""

processed_html = merge_empty_leading_tds(html)
print(processed_html)
