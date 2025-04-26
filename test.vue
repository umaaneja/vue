DOCX_TEMPLATES = {
    "Columns": {
        "layout": "2-column table",
        "styles": {
            "header": "Heading 2",
            "column_border": "0.5pt solid #ddd"
        },
        "content_map": {
            "left": {"type": "richtext", "style": "Normal"},
            "right": {"type": "media+text", "style": "Caption"}
        }
    },
    "Cards": {
        "layout": "n-column table",
        "styles": {
            "header": "Heading 3",
            "card_border": "1pt solid #eee"
        },
        "content_map": {
            "image": {"width": "1.35in", "height": "auto"},
            "text": {"style": "Normal", "bold_first_line": True}
        }
    }
}
