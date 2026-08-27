# Module 2 — Develop a RESTful Service Using Test-Driven Development

## Completion status

**Repository implementation:** completed in the working tree.
**Full Nose + coverage verification:** pending execution in the remote lab because this test suite is configured to use PostgreSQL. No Docker container, database volume, service process, or dependency installation was created for this result.

## Implemented deliverables

| Module 2 requirement                                     | Repository evidence                                   | Status   |
| -------------------------------------------------------- | ----------------------------------------------------- | -------- |
| Configure Nose defaults                                  | [`setup.cfg`](../../setup.cfg)                       | Complete |
| List accounts:`GET /accounts` → 200                   | [`service/routes.py`](../../service/routes.py)       | Complete |
| Read account:`GET /accounts/<id>` → 200 / 404         | [`service/routes.py`](../../service/routes.py)       | Complete |
| Update account:`PUT /accounts/<id>` → 200 / 404 / 415 | [`service/routes.py`](../../service/routes.py)       | Complete |
| Delete account:`DELETE /accounts/<id>` → 204          | [`service/routes.py`](../../service/routes.py)       | Complete |
| CRUD and error-path route tests                          | [`tests/test_routes.py`](../../tests/test_routes.py) | Complete |
| Create route Location header points to read route        | [`service/routes.py`](../../service/routes.py)       | Complete |

## Lightweight validation completed

The following commands passed without creating infrastructure:

```text
uv run --no-project python -m compileall -q service tests
uv run --no-project python -m py_compile service/routes.py tests/test_routes.py
uv run --no-project python -c '<verify required Nose settings>'
git diff --check
```

The configuration verification confirmed these required Nose settings:

```ini
[nosetests]
verbosity=2
with-spec=1
spec-color=1
with-coverage=1
cover-erase=1
cover-package=service
```

## Required remote-lab verification

Run these only in the supplied Cloud IDE after its environment and PostgreSQL service are ready:

```bash
bash ./bin/setup.sh
exit
# Open a new terminal so the lab virtual environment is active.
flask db-create
nosetests
coverage report -m
```

Expected completion criteria:

- All Nose tests pass.
- Coverage is **95% or higher** for the `service` package.
- Resolve any failed assertion or missed coverage line before creating the pull request.

Do not use `make db`, `make build`, `make cluster`, `make tekton`, or `make clustertasks` merely to validate this module. Those commands allocate infrastructure or storage and are not needed if the lab already provides PostgreSQL.

## Git workflow to finish

Use separate feature branches and pull requests if your instructor expects the full TDD history. If you are submitting the completed working tree as one change, use a single feature branch:

```bash
git checkout -b module-2-rest-api
git add setup.cfg service/routes.py tests/test_routes.py 10_docs
git commit -m "Implement account CRUD REST API"
git push --set-upstream origin module-2-rest-api
```

Then create a GitHub pull request into `main`, confirm the tests/coverage result, merge it, and delete the branch.

## Kanban work required in GitHub

These actions cannot be completed from the repository; perform them in your public GitHub Project board:

1. Assign each story to yourself before implementation.
2. Move **Set up the development environment** to **Done** after the `setup.cfg` change is merged.
3. Move **Read an account from the service** to **Done** after the read route and tests pass.
4. Move **List all accounts in the service** to **Done** after the list route and tests pass.
5. Move **Update an account in the service** to **Done** after the update route and tests pass.
6. Move **Delete an account in the service** to **Done** after the delete route and tests pass.

## Required evidence and how to capture it

### Q7 — public `setup.cfg` URL

After merging to `main`, submit:

```text
https://github.com/johnny-official/devops-capstone-project/blob/main/setup.cfg
```

No image is required for the AI-graded option. For peer grading, capture the Cloud IDE editor with the complete `setup.cfg` content visible and save it as:

```text
rest-setupcfg-done.png
```

### Q8–Q12 — Kanban screenshots

Capture your GitHub Project board immediately after moving each card to **Done**. Include the project title, the Done column heading, and the complete card title. Use PNG and preserve these exact names:

| Task | Story visible in Done                  | Required filename          |
| ---- | -------------------------------------- | -------------------------- |
| Q8   | Setting up the development environment | `rest-techdebt-done.png` |
| Q9   | Read an account from the service       | `read-accounts.png`      |
| Q10  | List all accounts in the service       | `list-accounts.png`      |
| Q11  | Update an account in the service       | `update-accounts.png`    |
| Q12  | Delete an account in the service       | `delete-accounts.png`    |

**Capture procedure:** open the board in a desktop browser, zoom to 90–100%, make the Done column readable, hide unrelated personal browser tabs/profile details, and take one screenshot per named story. Do not use staged or fabricated screenshots.

### Q13–Q17 — cURL text evidence

With `make run` already running in the remote lab, use a second terminal. Replace `<ID>` with the ID returned by create. Copy both the command and actual output into the corresponding plain-text submission answer.

```bash
# CREATE — save as rest-create-done
curl -i -X POST http://127.0.0.1:5000/accounts \
  -H "Content-Type: application/json" \
  -d '{"name":"John Doe","email":"john@doe.com","address":"123 Main St.","phone_number":"555-1212"}'

# LIST — save as rest-list-done
curl -i -X GET http://127.0.0.1:5000/accounts

# READ — save as rest-read-done
curl -i -X GET http://127.0.0.1:5000/accounts/<ID>

# UPDATE — save as rest-update-done
curl -i -X PUT http://127.0.0.1:5000/accounts/<ID> \
  -H "Content-Type: application/json" \
  -d '{"name":"John Doe","email":"john@doe.com","address":"123 Main St.","phone_number":"555-1111"}'

# DELETE — save as rest-delete-done
curl -i -X DELETE http://127.0.0.1:5000/accounts/<ID>
```

Expected statuses: create `201`, list `200`, read `200`, update `200`, delete `204`.

For peer grading, take a separate terminal screenshot for each command and save it as `rest-create-done.png`, `rest-list-done.png`, `rest-read-done.png`, `rest-update-done.png`, and `rest-delete-done.png`.

## Final pre-submission checklist

- [ ] `nosetests` passes in the remote lab.
- [ ] `coverage report -m` reports at least 95% for `service`.
- [ ] Changes are pushed and merged to the public `main` branch.
- [ ] Q7 public URL loads while signed out.
- [ ] Five Kanban screenshots are captured using the exact filenames.
- [ ] Five cURL commands and real outputs are copied to the required submission fields.
- [ ] No token, password, private hostname, or personal data appears in any capture.
