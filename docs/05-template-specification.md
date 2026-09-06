# Template Specification


Template یک Lab آموزشی قابل اجرا است.


ساختار:
Templates/nginx-basic/


template.yaml

manifests/

tasks/

tests/


---

# Template باید شامل:


- Metadata
- Architecture
- Difficulty
- Resources
- Tasks
- Validation
- Documentation


باشد.


---

# Example


name: nginx-basic

difficulty: easy

duration: 30m

monitoring:
 enabled: true
