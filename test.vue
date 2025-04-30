from bs4 import BeautifulSoup

def merge_empty_leading_tds(html_content):
    """Merges empty leading TD cells into the first non-empty TD with colspan"""
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
            
        # Get content and attributes from first non-empty td
        content = tds[first_non_empty].decode_contents()
        attrs = tds[first_non_empty].attrs
        
        # Create new td with colspan and original attributes
        attrs['colspan'] = str(first_non_empty + 1)
        new_td = soup.new_tag('td', **attrs)
        new_td.append(BeautifulSoup(content, 'html.parser'))
        
        # Replace all tds up to first non-empty with our new td
        for td in tds[:first_non_empty + 1]:
            td.decompose()
            
        first_row.insert(0, new_td)
                
    return str(soup)

def process_html_file(input_file, output_file):
    """Reads HTML from input file, processes it, and writes to output file"""
    try:
        with open(input_file, 'r', encoding='utf-8') as f:
            html = f.read()
        
        processed_html = merge_empty_leading_tds(html)
        
        with open(output_file, 'w', encoding='utf-8') as f:
            f.write(processed_html)
        
        print(f"Successfully processed HTML. Output saved to {output_file}")
    except Exception as e:
        print(f"Error processing file: {e}")

# Example usage
if __name__ == "__main__":
    input_filename = "input.html"  # Change to your input file
    output_filename = "output.html"  # Change to your output file
    
    process_html_file(input_filename, output_filename)
