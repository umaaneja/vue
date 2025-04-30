   pypandoc.convert_text(
        bordered_html,
        'docx',
        format='html',
        outputfile=docx_file,
        extra_args=['--reference-doc=template.docx']  # Optional template
    )
