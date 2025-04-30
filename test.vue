from bs4 import BeautifulSoup

def merge_empty_tds(html_content):
    soup = BeautifulSoup(html_content, 'html.parser')
    
    for table in soup.find_all('table'):
        rows = table.find_all('tr')
        if not rows:
            continue
            
        for row in rows:
            tds = row.find_all('td')
            if not tds:
                continue
                
            # Find first and last non-empty tds
            first_non_empty = None
            last_non_empty = None
            for i, td in enumerate(tds):
                if td.get_text(strip=True):
                    if first_non_empty is None:
                        first_non_empty = i
                    last_non_empty = i
            
            if first_non_empty is None:
                continue  # All cells are empty
                
            # Process empty cells before first non-empty
            if first_non_empty > 0:
                content = tds[first_non_empty].decode_contents()
                attrs = tds[first_non_empty].attrs
                attrs['colspan'] = str(first_non_empty + 1)
                new_td = soup.new_tag('td', **attrs)
                new_td.append(BeautifulSoup(content, 'html.parser'))
                
                for td in tds[:first_non_empty + 1]:
                    td.decompose()
                    
                row.insert(0, new_td)
                tds = row.find_all('td')  # Refresh after modification
            
            # Process empty cells after last non-empty
            if last_non_empty < len(tds) - 1:
                content = tds[last_non_empty].decode_contents()
                attrs = tds[last_non_empty].attrs
                span = len(tds) - last_non_empty
                if 'colspan' in attrs:
                    span += int(attrs['colspan']) - 1
                attrs['colspan'] = str(span)
                new_td = soup.new_tag('td', **attrs)
                new_td.append(BeautifulSoup(content, 'html.parser'))
                
                for td in tds[last_non_empty:]:
                    td.decompose()
                    
                row.append(new_td)
                
    return str(soup)

# Test cases
html1 = """
<table>
  <tr>
    <td></td>
    <td></td>
    <td>Actual Content</td>
  </tr>
</table>
"""

html2 = """
<table>
  <tr>
    <td>Actual Content</td>
    <td></td>
    <td></td>
  </tr>
</table>
"""

print("Test Case 1:")
print(merge_empty_tds(html1))
print("\nTest Case 2:")
print(merge_empty_tds(html2))
