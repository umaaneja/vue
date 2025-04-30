    """Converts HTML file to DOCX with preserved table borders"""
    with open(html_file, 'r', encoding='utf-8') as f:
        html = f.read()
    
    # Add explicit border styles
    bordered_html = add_table_borders(html)
