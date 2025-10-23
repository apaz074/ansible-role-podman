---

description: "Task list template for feature implementation"
---

# Tasks: Ansible Role for Podman Installation

**Input**: Design documents from `/specs/001-ansible-role-podman/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: The examples below include test tasks. Tests are OPTIONAL - only include them if explicitly requested in the feature specification.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **Single project**: `src/`, `tests/` at repository root
- **Web app**: `backend/src/`, `frontend/src/`
- **Mobile**: `api/src/`, `ios/src/` or `android/src/`
- Paths shown below assume single project - adjust based on plan.md structure

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and basic structure

- [ ] T001 Create basic Ansible role structure (defaults, tasks, handlers, vars, meta, tests) in `/home/apaz074/projects/ansible-role-podman/`
- [ ] T002 Configure Molecule for Podman driver and multi-OS testing in `/home/apaz074/projects/ansible-role-podman/molecule/default/molecule.yml`
- [ ] T003 Create Molecule `create.yml` playbook for Podman containers in `/home/apaz074/projects/ansible-role-podman/molecule/default/create.yml`
- [ ] T004 Create Molecule `destroy.yml` playbook for Podman containers in `/home/apaz074/projects/ansible-role-podman/molecule/default/destroy.yml`
- [ ] T005 Create Molecule `converge.yml` playbook to apply the role in `/home/apaz074/projects/ansible-role-podman/molecule/default/converge.yml`
- [ ] T006 Create Molecule `verify.yml` playbook for initial verification in `/home/apaz074/projects/ansible-role-podman/molecule/default/verify.yml`

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [ ] T007 Implement multi-OS package installation logic in `/home/apaz074/projects/ansible-role-podman/tasks/install.yml`
- [ ] T008 Implement multi-OS service management logic in `/home/apaz074/projects/ansible-role-podman/tasks/service.yml`
- [ ] T009 Include `install.yml` and `service.yml` in `/home/apaz074/projects/ansible-role-podman/tasks/main.yml`
- [ ] T010 Define default variables for `podman_package_name`, `podman_service_name` in `/home/apaz074/projects/ansible-role-podman/defaults/main.yml`
- [ ] T011 Create OS-specific variable files for Debian (`vars/Debian.yml`), RHEL (`vars/RedHat.yml`), and Arch Linux (`vars/Archlinux.yml`)
- [ ] T012 Implement graceful failure for unsupported operating systems in `/home/apaz074/projects/ansible-role-podman/tasks/main.yml`

**Checkpoint**: Foundation ready - user story implementation can now begin in parallel

---

## Phase 3: User Story 1 - Install Podman on a supported distribution (Priority: P1) 🎯 MVP

**Goal**: Ensure Podman is installed and its service is running on supported Linux distributions.

**Independent Test**: Run `molecule test` and verify that Podman is installed and `podman.socket` is active on Debian, RHEL, and Arch Linux instances.

### Implementation for User Story 1

- [ ] T013 [P] [US1] Implement package installation for Debian family in `/home/apaz074/projects/ansible-role-podman/tasks/install.yml` and `/home/apaz074/projects/ansible-role-podman/vars/Debian.yml`
- [ ] T014 [P] [US1] Implement package installation for RHEL family in `/home/apaz074/projects/ansible-role-podman/tasks/install.yml` and `/home/apaz074/projects/ansible-role-podman/vars/RedHat.yml`
- [ ] T015 [P] [US1] Implement package installation for Arch Linux family in `/home/apaz074/projects/ansible-role-podman/tasks/install.yml` and `/home/apaz074/projects/ansible-role-podman/vars/Archlinux.yml`
- [ ] T016 [P] [US1] Implement service management for Debian family in `/home/apaz074/projects/ansible-role-podman/tasks/service.yml`
- [ ] T017 [P] [US1] Implement service management for RHEL family in `/home/apaz074/projects/ansible-role-podman/tasks/service.yml`
- [ ] T018 [P] [US1] Implement service management for Arch Linux family in `/home/apaz074/projects/ansible-role-podman/tasks/service.yml`
- [ ] T019 [US1] Add Molecule test for Podman installation and service status on Debian in `/home/apaz074/projects/ansible-role-podman/molecule/default/verify.yml`
- [ ] T020 [US1] Add Molecule test for Podman installation and service status on RHEL in `/home/apaz074/projects/ansible-role-podman/molecule/default/verify.yml`
- [ ] T021 [US1] Add Molecule test for Podman installation and service status on Arch Linux in `/home/apaz074/projects/ansible-role-podman/molecule/default/verify.yml`
- [ ] T022 [US1] Add Molecule idempotency test for Podman installation in `/home/apaz074/projects/ansible-role-podman/molecule/default/verify.yml`

**Checkpoint**: At this point, User Story 1 should be fully functional and testable independently

---

## Phase 4: User Story 2 - Configure Registry Lists (Priority: P2)

**Goal**: Configure Podman's `registries.conf` using `registries_search`, `registries_insecure`, and `registries_block` variables.

**Independent Test**: Run `molecule test` and verify that `/etc/containers/registries.conf` is correctly populated with the specified registry lists on test instances.

### Implementation for User Story 2

- [ ] T023 [US2] Define `registries_search`, `registries_insecure`, `registries_block` variables in `/home/apaz074/projects/ansible-role-podman/defaults/main.yml`
- [ ] T024 [P] [US2] Implement `registries.conf` generation using `registries_search` in `/home/apaz074/projects/ansible-role-podman/tasks/configure_registries.yml`
- [ ] T025 [P] [US2] Implement `registries.conf` generation using `registries_insecure` in `/home/apaz074/projects/ansible-role-podman/tasks/configure_registries.yml`
- [ ] T026 [P] [US2] Implement `registries.conf` generation using `registries_block` in `/home/apaz074/projects/ansible-role-podman/tasks/configure_registries.yml`
- [ ] T027 [US2] Include `configure_registries.yml` in `/home/apaz074/projects/ansible-role-podman/tasks/main.yml`
- [ ] T028 [US2] Implement URI validation logic for registry entries in `/home/apaz074/projects/ansible-role-podman/tasks/configure_registries.yml`
- [ ] T029 [US2] Add Molecule test for `registries_search` configuration in `/home/apaz074/projects/ansible-role-podman/molecule/default/verify.yml`
- [ ] T030 [US2] Add Molecule test for `registries_insecure` configuration in `/home/apaz074/projects/ansible-role-podman/molecule/default/verify.yml`
- [ ] T031 [US2] Add Molecule test for `registries_block` configuration in `/home/apaz074/projects/ansible-role-podman/molecule/default/verify.yml`
- [ ] T032 [US2] Add Molecule test for invalid registry URI handling in `/home/apaz074/projects/ansible-role-podman/molecule/default/verify.yml`
- [ ] T033 [US2] Add Molecule idempotency test for registry configuration in `/home/apaz074/projects/ansible-role-podman/molecule/default/verify.yml`

**Checkpoint**: All user stories should now be independently functional

---

## Phase N: Polish & Cross-Cutting Concerns

**Purpose**: Improvements that affect multiple user stories

- [ ] T034 [P] Update `README.md` with input variables documentation in `/home/apaz074/projects/ansible-role-podman/README.md`
- [ ] T035 [P] Run `ansible-lint` and `yamllint` to ensure code quality across the role
- [ ] T036 Code cleanup and refactoring for the entire role
- [ ] T037 Run `ruff check .` for Python code linting in `/home/apaz074/projects/ansible-role-podman/`
- [ ] T038 Final review of all documentation and code

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3+)**: All depend on Foundational phase completion
  - User stories can then proceed in parallel (if staffed)
  - Or sequentially in priority order (P1 → P2 → P3)
- **Polish (Final Phase)**: Depends on all desired user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2) - No dependencies on other stories
- **User Story 2 (P2)**: Can start after Foundational (Phase 2) - May integrate with US1 but should be independently testable

### Within Each User Story

- Tests (if included) MUST be written and FAIL before implementation
- Models before services
- Services before endpoints
- Core implementation before integration
- Story complete before moving to next priority

### Parallel Opportunities

- All Setup tasks marked [P] can run in parallel
- All Foundational tasks marked [P] can run in parallel (within Phase 2)
- Once Foundational phase completes, all user stories can start in parallel (if team capacity allows)
- All tests for a user story marked [P] can run in parallel
- Models within a story marked [P] can run in parallel
- Different user stories can be worked on in parallel by different team members

---

## Parallel Example: User Story 1

```bash
# Launch all tests for User Story 1 together (if tests requested):
Task: "Add Molecule test for Podman installation and service status on Debian in /home/apaz074/projects/ansible-role-podman/molecule/default/verify.yml"
Task: "Add Molecule test for Podman installation and service status on RHEL in /home/apaz074/projects/ansible-role-podman/molecule/default/verify.yml"
Task: "Add Molecule test for Podman installation and service status on Arch Linux in /home/apaz074/projects/ansible-role-podman/molecule/default/verify.yml"

# Launch all implementation tasks for User Story 1 that are parallelizable:
Task: "Implement package installation for Debian family in /home/apaz074/projects/ansible-role-podman/tasks/install.yml and /home/apaz074/projects/ansible-role-podman/vars/Debian.yml"
Task: "Implement package installation for RHEL family in /home/apaz074/projects/ansible-role-podman/tasks/install.yml and /home/apaz074/projects/ansible-role-podman/vars/RedHat.yml"
Task: "Implement package installation for Arch Linux family in /home/apaz074/projects/ansible-role-podman/tasks/install.yml and /home/apaz074/projects/ansible-role-podman/vars/Archlinux.yml"
Task: "Implement service management for Debian family in /home/apaz074/projects/ansible-role-podman/tasks/service.yml"
Task: "Implement service management for RHEL family in /home/apaz074/projects/ansible-role-podman/tasks/service.yml"
Task: "Implement service management for Arch Linux family in /home/apaz074/projects/ansible-role-podman/tasks/service.yml"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational (CRITICAL - blocks all stories)
3. Complete Phase 3: User Story 1
4. **STOP and VALIDATE**: Test User Story 1 independently
5. Deploy/demo if ready

### Incremental Delivery

1. Complete Setup + Foundational → Foundation ready
2. Add User Story 1 → Test independently → Deploy/Demo (MVP!)
3. Add User Story 2 → Test independently → Deploy/Demo
4. Each story adds value without breaking previous stories

### Parallel Team Strategy

With multiple developers:

1. Team completes Setup + Foundational together
2. Once Foundational is done:
   - Developer A: User Story 1
   - Developer B: User Story 2
3. Stories complete and integrate independently

---

## Notes

- [P] tasks = different files, no dependencies
- [Story] label maps task to specific user story for traceability
- Each user story should be independently completable and testable
- Verify tests fail before implementing
- Commit after each task or logical group
- Stop at any checkpoint to validate story independently
- Avoid: vague tasks, same file conflicts, cross-story dependencies that break independence