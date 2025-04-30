import re

def process_tables(html_content):
    # Split the HTML into parts before, between, and after tables
    parts = re.split(r'(<table[^>]*>.*?</table>)', html_content, flags=re.DOTALL)
    
    for i in range(len(parts)):
        if not parts[i].strip().startswith('<table'):
            continue
            
        # Process each table
        table_content = parts[i]
        # Find first <tr> in the table
        tr_match = re.search(r'<tr[^>]*>(.*?)</tr>', table_content, re.DOTALL)
        if not tr_match:
            continue
            
        tr_content = tr_match.group(1)
        # Find all <td> elements in the first <tr>
        tds = re.findall(r'<td[^>]*>(.*?)</td>', tr_content, re.DOTALL)
        
        if not tds:
            continue
            
        # Find last non-empty td
        last_non_empty = -1
        for j in range(len(tds)):
            if tds[j].strip():
                last_non_empty = j
                
        if last_non_empty == -1:
            continue  # all tds are empty
            
        # Process empty tds before the last non-empty one
        for j in range(last_non_empty):
            if not tds[j].strip():
                # Replace empty td with colspan
                colspan = last_non_empty - j + 1
                empty_td_pattern = r'(<td[^>]*>)' + re.escape(tds[j]) + r'(</td>)'
                replacement = f'<td colspan="{colspan}"></td>'
                tr_content = re.sub(empty_td_pattern, replacement, tr_content, flags=re.DOTALL)
                
                # Update the table content with modified tr
                table_content = table_content[:tr_match.start(1)] + tr_content + table_content[tr_match.end(1):]
                
        # Update the part with modified table
        parts[i] = table_content
        
    # Rejoin all parts
    return ''.join(parts)

# Example usage:
with open('your_file.html', 'r', encoding='utf-8') as f:
    html = f.read()

processed_html = process_tables(html)

with open('processed_file.html', 'w', encoding='utf-8') as f:
    f.write(processed_html)
