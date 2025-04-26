<thinking>
1. TEMPLATE MATCHING:
   - Compare each visual element against: 
     {json.dumps(template['visual_descriptors'], indent=2)}
   - Score similarity for known component patterns (0-1)
   - Flag elements not matching expected patterns

2. CONTENT EXTRACTION:
   - Extract all visible text with semantic labeling
   - Identify images/icons and their contextual meaning
   - Capture interactive element states/labels

3. TEMPLATE VALIDATION:
   - Verify required components are present
   - Check component content against template rules
   - Document all deviations from expected patterns
</thinking>

<answer format="json">
{
  "template_analysis": {
    "template_id": "homepage_v3",
    "components": [
      {
        "component_type": "hero_banner",
        "template_match": {
          "expected": "Full-width image with centered text overlay",
          "match_confidence": 0.97,
          "content": {
            "heading": "Welcome to Our Platform",
            "subtext": "Discover innovative solutions",
            "cta_button": "Explore Now"
          }
        }
      },
      {
        "component_type": "product_grid",
        "template_match": {
          "expected": "3-column card layout with product details",
          "match_confidence": 0.89,
          "content": {
            "items": [
              {
                "title": "Premium Plan",
                "price": "$99/month"
              }
            ]
          }
        }
      }
    ],
    "validation": {
      "missing_components": ["testimonial_section"],
      "content_issues": [
        {
          "component": "pricing_table",
          "issue": "Discount label missing",
          "required_by_template": true
        }
      ]
    }
  }
}
</answer>
