# Issue #016: Role Definitions

**Milestone:** Milestone 3 - RBAC Staff Roles  
**Phase:** SDLC Phase 1 - Requirements  
**Status:** ✅ Complete  
**Owner:** ANSVK  
**Created:** 2026-11-25  
**Started:** 2026-11-25  
**Completed:** 2026-11-25

---

## Objective

Define the 7 staff roles for the RBAC system, documenting each role's responsibilities, typical job functions, access needs, and relationship to clinic operations.

---

## Scope

**In Scope:**
- Definition of 7 roles: Admin, Doctor, Nurse, Pharmacist, Receptionist, Peer Counselor, Manager
- Each role's job responsibilities and functions
- Each role's clinical vs administrative access needs
- Typical use cases and workflows for each role
- Hierarchy and reporting relationships (documentation only, not implementation)

**Out of Scope:**
- Detailed permission matrix (covered in #017)
- Technical implementation (Phase 4)
- Role assignment procedures (Phase 6 - Deployment)
- Multi-role assignments (future enhancement)

---

## Role Definitions

### Role 1: Administrator (Admin)

**Job Function:**
- System administrator and technical support
- User management and system configuration
- Database maintenance and troubleshooting
- Security and compliance oversight

**Access Level:** Full system access

**Responsibilities:**
- Create, modify, and disable user accounts
- Assign users to appropriate roles
- Configure system settings and permissions
- Monitor system health and security
- Perform database backups and maintenance
- Review audit logs for security compliance
- Handle technical support escalations

**Typical Users:**
- IT administrator
- System administrator
- Technical support staff

**Access Needs:**
- Full CRUDE access to all resources
- User management functions
- System configuration access
- Audit log access
- Database administration tools

**Restrictions:**
- None (full access)

---

### Role 2: Doctor (Physician)

**Job Function:**
- Primary medical care provider
- Diagnosis and treatment planning
- Prescription management
- Medical record documentation

**Access Level:** Full clinical access

**Responsibilities:**
- Register and manage patients
- Document patient encounters and medical history
- Review and update patient medical records
- Prescribe medications
- Order and review diagnostic tests
- Document treatment plans and progress notes
- Review patient data for continuity of care

**Typical Users:**
- Medical Doctor (MD)
- Doctor of Osteopathic Medicine (DO)
- Physician Assistant (in some settings)

**Access Needs:**
- Full CRUDE access to patient clinical data
- Create/Read/Update patient demographics
- Create/Read/Update medical records
- Create/Read prescriptions
- Read pharmacy dispensing records
- Export patient data (FHIR)

**Restrictions:**
- Cannot manage users or system configuration
- Cannot modify audit logs
- Cannot access other users' credentials

---

### Role 3: Nurse

**Job Function:**
- Patient care delivery and monitoring
- Medication administration
- Vital signs and clinical documentation
- Patient education and care coordination

**Access Level:** Clinical access with limited prescribing

**Responsibilities:**
- Document patient vital signs and assessments
- Record medication administration
- Update patient care plans
- Document nursing notes and observations
- Assist with patient registration (demographics)
- Monitor patient status and progress
- Document patient education provided

**Typical Users:**
- Registered Nurse (RN)
- Licensed Practical Nurse (LPN)
- Licensed Vocational Nurse (LVN)

**Access Needs:**
- Create/Read/Update patient clinical data (not delete)
- Read patient demographics
- Create/Update nursing documentation
- Read prescriptions and medication orders
- Read/Update care plans
- Limited create prescriptions (standing orders only, if applicable)

**Restrictions:**
- Cannot delete patient records
- Cannot register new patients (can update demographics)
- Cannot manage users or system settings
- Cannot access financial/billing data
- Cannot modify audit logs

---

### Role 4: Pharmacist

**Job Function:**
- Medication dispensing and verification
- Drug interaction checking
- Prescription review and counseling
- Pharmacy inventory management

**Access Level:** Pharmacy-focused access

**Responsibilities:**
- Review and verify prescriptions
- Document medication dispensing
- Check drug interactions and allergies
- Counsel patients on medication use
- Review patient medication history
- Document pharmacy notes
- Monitor medication adherence

**Typical Users:**
- Licensed Pharmacist (PharmD, RPh)
- Pharmacy Technician (limited access)

**Access Needs:**
- Read patient demographics and allergies
- Read prescriptions and medication orders
- Create/Read/Update dispensing records
- Read relevant medical history (allergies, diagnoses)
- Create pharmacy notes/counseling documentation
- Read medication history

**Restrictions:**
- Cannot create or modify prescriptions (read only)
- Cannot access full medical records (limited to medication-relevant data)
- Cannot register patients or modify demographics
- Cannot delete patient records
- Cannot manage users or system settings
- No access to non-medication clinical notes

---

### Role 5: Receptionist

**Job Function:**
- Patient registration and check-in
- Appointment scheduling
- Demographic data management
- Front desk operations

**Access Level:** Administrative/demographic access with limited clinical view

**Responsibilities:**
- Register new patients
- Update patient demographic information
- Verify patient identity and insurance
- Schedule appointments (future: not in Milestone 3 scope)
- Check patients in/out
- Handle basic patient inquiries
- Maintain accurate contact information

**Typical Users:**
- Front desk receptionist
- Registration clerk
- Administrative assistant

**Access Needs:**
- Create/Read/Update patient demographics
- Read patient basic info (name, DOB, MRN)
- Create new patient records
- Update contact information
- Read-only limited clinical access (verify patient identity)
- Search patients

**Restrictions:**
- Cannot access detailed clinical notes
- Cannot view sensitive medical history
- Cannot access prescriptions or medications
- Cannot delete patient records (can deactivate)
- Cannot manage users or system settings
- Cannot access audit logs

---

### Role 6: Peer Counselor

**Job Function:**
- Provide peer support and counseling services
- Mental health and substance abuse support
- Patient education and advocacy
- Community health outreach

**Access Level:** Limited clinical access to assigned patients

**Responsibilities:**
- Document peer counseling sessions
- Review assigned patient information
- Create counseling notes and plans
- Document patient education provided
- Track patient progress in counseling programs
- Coordinate with clinical team

**Typical Users:**
- Certified Peer Support Specialist
- Community Health Worker
- Patient Navigator
- Recovery Coach

**Access Needs:**
- Read assigned patient demographics
- Read assigned patient relevant history (substance use, mental health)
- Create/Read/Update counseling notes
- Create/Update patient education documentation
- Read care plans related to counseling

**Restrictions:**
- Access limited to assigned patients only (future: Milestone 3 uses resource-type permissions)
- Cannot access full medical records (limited to counseling-relevant data)
- Cannot prescribe medications
- Cannot delete patient records
- Cannot register new patients (can read demographics)
- Cannot manage users or system settings
- No access to unrelated clinical data (lab results, prescriptions)

---

### Role 7: Manager

**Job Function:**
- Clinical operations management
- Quality assurance and reporting
- Staff oversight and coordination
- Compliance and performance monitoring

**Access Level:** Read-mostly access with reporting functions

**Responsibilities:**
- Review patient records for quality assurance
- Generate reports and analytics
- Monitor clinic operations and workflows
- Review staff documentation for completeness
- Oversee compliance with policies
- Access audit logs for operational review
- Coordinate care team activities

**Typical Users:**
- Clinic Manager
- Nursing Supervisor
- Quality Assurance Coordinator
- Operations Manager

**Access Needs:**
- Read all patient data (no modify)
- Read user information (not credentials)
- Read audit logs
- Generate reports and analytics
- Read system configuration (not modify)
- Export data for reporting

**Restrictions:**
- Cannot create, update, or delete patient records (read only)
- Cannot manage users or change roles (read only)
- Cannot modify system configuration
- Cannot access user credentials
- Cannot modify audit logs (read only)
- Cannot prescribe or dispense medications

---

## Role Comparison Matrix

| Aspect | Admin | Doctor | Nurse | Pharmacist | Receptionist | Peer Counselor | Manager |
|--------|-------|--------|-------|------------|--------------|----------------|---------|
| **Primary Focus** | System | Clinical | Clinical | Pharmacy | Administrative | Counseling | Operations |
| **Patient Create** | ✅ | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ |
| **Demographics Access** | ✅ Full | ✅ Full | 👁️ Read | 👁️ Limited | ✅ Full | 👁️ Read | 👁️ Read |
| **Clinical Notes** | ✅ Full | ✅ Full | ✅ Create/Update | 👁️ Limited | ❌ | 👁️ Limited | 👁️ Read |
| **Prescriptions** | ✅ Full | ✅ Create/Read | 👁️ Read | 👁️ Read | ❌ | ❌ | 👁️ Read |
| **User Management** | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | 👁️ Read |
| **Audit Logs** | ✅ Full | ❌ | ❌ | ❌ | ❌ | ❌ | 👁️ Read |
| **System Config** | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | 👁️ Read |

**Legend:**
- ✅ Full access (CRUDE)
- 👁️ Read-only or limited access
- ❌ No access

---

## Hierarchy & Reporting (Documentation)

**Note:** This documents typical clinic hierarchy for context. Milestone 3 implements flat roles (no role inheritance).

```
Clinic Manager (Manager role)
├── Doctor (Doctor role)
├── Nurse (Nurse role)
├── Pharmacist (Pharmacist role)
├── Peer Counselor (Peer Counselor role)
└── Receptionist (Receptionist role)

Administrator (Admin role) - Technical, not in clinical hierarchy
```

---

## Use Case Examples

### Use Case 1: New Patient Registration
1. **Receptionist** creates patient record with demographics
2. **Doctor** or **Nurse** adds clinical information during visit
3. **Doctor** documents encounter and creates prescription
4. **Pharmacist** reviews prescription and dispenses medication
5. **Manager** can review all documentation for quality assurance

### Use Case 2: Patient Follow-up Visit
1. **Receptionist** checks patient in (verifies demographics)
2. **Nurse** records vital signs and updates patient status
3. **Doctor** reviews history, updates treatment plan
4. **Peer Counselor** (if applicable) documents counseling session
5. **Manager** reviews documentation completeness

### Use Case 3: System Administration
1. **Admin** creates new user account for new staff member
2. **Admin** assigns appropriate role (Doctor, Nurse, etc.)
3. **Manager** can verify user has been created (read-only)
4. Staff member logs in with assigned role permissions

---

## Success Criteria

- [x] All 7 roles defined with clear job functions
- [x] Each role's responsibilities documented
- [x] Each role's access needs specified
- [x] Each role's restrictions identified
- [x] Role comparison matrix created
- [x] Use case examples provided
- [x] Hierarchy documented (for context)
- [x] Reviewed for completeness
- [x] Validated against typical healthcare clinic structure
- [x] Approved by project owner (ANSVK) ✅ **APPROVED 2026-11-25**

---

## Dependencies

**Completed:**
- ✅ Issue #015: RBAC overview requirements

**Pending:**
- 🔴 Issue #017: Permission matrix (detailed CRUDE mapping)
- 🔴 Issue #018: RBAC policy document
- 🔴 Issue #019-022: Analysis phase

**Blocks:**
- Issue #017: Permission matrix depends on these role definitions

---

## Deliverables

- [x] This role definitions document
- [x] 7 roles completely defined
- [x] Role comparison matrix
- [x] Use case examples
- [ ] Validation against research (from kickoff meeting)

---

## Notes

**Research Alignment:**
These role definitions are based on the research documented in `meetings/2026-11-25-milestone-3-kickoff.md`:
- Typical healthcare role permissions from industry sources
- "Need to know" principle (minimum necessary access)
- HL7 FHIR resource-based permissions model
- HIPAA minimum necessary standard

**Design Decisions:**
- Roles are mutually exclusive (one role per user in Milestone 3)
- Resource-type permissions (all patients, not per-patient in Milestone 3)
- Peer Counselor access documented as "assigned patients" but Milestone 3 implements resource-type (future enhancement)
- Manager is read-only to enforce separation of duties
- Admin has full access as "super user" for system maintenance

---

## Status History

- 2026-11-25 11:15: Created (🟡 In Progress)
- 2026-11-25 11:30: All 7 roles documented
- 2026-11-25 11:45: Approved by ANSVK (✅ Complete)