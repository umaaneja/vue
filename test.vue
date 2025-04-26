def get_template_specific_prompt(template_id, image_bytes):
    template = template_registry[template_id]
    
    prompt = f"""
    Analyze this AEM page screenshot for template: {template_id}.
    Expected component types and characteristics:
    {json.dumps(template['visual_descriptors'], indent=2)}
    
    For each detected component:
    1. Match to nearest expected component type
    2. Provide confidence score (0-1)
    3. Note any visual deviations from template standard
    4. Extract all editable text content
    
    Return JSON with this structure:
    {{
        "components": [{{
            "matched_component": "ComponentName",
            "confidence": 0.95,
            "position": {{"x","y","w","h"}},
            "content_elements": ["text1", "text2"],
            "visual_anomalies": ["unexpected_color"]
        }}]
    }}
    """
    return prompt
