# QA Notes

**Project:** Visitor Check-in Application  
**Assessment:** QA Assessment – Chulo Solutions / Namespace Inc.  
**Tester:** Puja Shah  

## Highest-Risk Area

The highest-risk area is the visitor registration and active visitor lifecycle.

This area is important because incorrect registration data can create invalid visitor records, and the active visitor list is used by receptionists to know who is currently inside the office.

During testing, issues were found around:
- Required field validation
- Whitespace-only Full Name
- Very long Full Name input causing an application crash
- Active visitor pagination
- Checked-in time display
- Duplicate submission

The timezone issue is particularly important because the requirements explicitly state that times must be displayed in the receptionist's local timezone.

## Product Owner Question Before Sign-off

Should Full Name have specific character-validation rules, such as allowing only letters and spaces, or should numbers and special characters also be allowed?

The current requirements do not define this behavior clearly, so the expected validation should be confirmed before treating those cases as confirmed defects.

## Additional Note

Company and Purpose also appear to support free-text values, but the requirements do not specify character restrictions. These behaviors should be confirmed with the Product Owner rather than assuming restrictions that are not documented.