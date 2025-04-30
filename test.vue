from html.parser import HTMLParser
import io

class TableProcessor(HTMLParser):
    def __init__(self):
        super().__init__()
        self.output = io.StringIO()
        self.in_table = False
        self.in_first_tr = False
        self.in_td = False
        self.current_td_content = ""
        self.td_contents = []
        self.process_first_tr = False

    def handle_starttag(self, tag, attrs):
        if tag == 'table':
            self.in_table = True
            self.process_first_tr = True
        elif tag == 'tr' and self.in_table and self.process_first_tr:
            self.in_first_tr = True
            self.td_contents = []
        elif tag == 'td' and self.in_first_tr:
            self.in_td = True
            self.current_td_content = ""
            
        # Write the tag to output
        attrs_str = ' '.join(f'{k}="{v}"' for k, v in attrs)
        self.output.write(f'<{tag}{" " + attrs_str if attrs_str else ""}>')

    def handle_endtag(self, tag):
        if tag == 'table':
            self.in_table = False
        elif tag == 'tr' and self.in_first_tr:
            self.in_first_tr = False
            self.process_first_tr = False
            
            # Process empty TDs
            last_non_empty = -1
            for i, content in enumerate(self.td_contents):
                if content.strip():
                    last_non_empty = i
            
            if last_non_empty != -1:
                # Go back and modify the TDs
                pos = self.output.tell()
                self.output.seek(0)
                content = self.output.getvalue()
                
                # Find all TDs in this TR
                td_matches = list(re.finditer(r'<td[^>]*>(.*?)</td>', content, re.DOTALL))
                
                for i in range(last_non_empty):
                    if not self.td_contents[i].strip():
                        # Calculate colspan
                        colspan = last_non_empty - i + 1
                        
                        # Replace the TD
                        match = td_matches[i]
                        new_td = f'<td colspan="{colspan}"></td>'
                        content = content[:match.start()] + new_td + content[match.end():]
                
                self.output = io.StringIO()
                self.output.write(content)
                self.output.seek(0, 2)  # Move to end
                
        elif tag == 'td' and self.in_first_tr:
            self.in_td = False
            self.td_contents.append(self.current_td_content)
            
        self.output.write(f'</{tag}>')

    def handle_data(self, data):
        if self.in_td:
            self.current_td_content += data
        self.output.write(data)

def fix_table_colspans(html_content):
    parser = TableProcessor()
    parser.feed(html_content)
    return parser.output.getvalue()

# Example usage
with open('input.html', 'r', encoding='utf-8') as f:
    html = f.read()

processed_html = fix_table_colspans(html)

with open('output.html', 'w', encoding='utf-8') as f:
    f.write(processed_html)
