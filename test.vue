from docx import Document
from docx.oxml import OxmlElement
from docx.oxml.ns import qn

def add_table_borders(docx_file, output_file):
    """Add borders to all tables in a DOCX file"""
    doc = Document(docx_file)
    
    # Function to set cell borders
    def set_cell_borders(cell):
        tc = cell._tc
        tcPr = tc.get_or_add_tcPr()
        
        # Create border elements
        for border in ['top', 'left', 'bottom', 'right']:
            tag = f'w:{border}'
            element = OxmlElement(tag)
            element.set(qn('w:val'), 'single')
            element.set(qn('w:sz'), '4')  # 0.5pt border
            element.set(qn('w:space'), '0')
            element.set(qn('w:color'), '000000')  # Black
            tcPr.append(element)
    
    # Process all tables
    for table in doc.tables:
        for row in table.rows:
            for cell in row.cells:
                set_cell_borders(cell)
    
    doc.save(output_file)
