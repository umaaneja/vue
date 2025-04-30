from docx import Document
from docx.shared import Pt

def create_template():
    doc = Document()
    
    # Set default paragraph properties
    style = doc.styles['Normal']
    font = style.font
    font.name = 'Calibri'
    font.size = Pt(11)
    
    # Add a sample table with borders (to establish the style)
    table = doc.add_table(rows=1, cols=1)
    table.style = 'Table Grid'  # This style has borders by default
    table.cell(0, 0).text = "This establishes border styles"
    
    doc.save('template.docx')

create_template()
