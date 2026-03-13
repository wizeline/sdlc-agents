# Security Patterns — Language-Specific Reference

## Python
**Dangerous calls to flag:**
- `eval()`, `exec()`, `compile()` with user input
- `pickle.loads()`, `yaml.load()` (use `yaml.safe_load()`)
- `subprocess.call(..., shell=True)` with user input
- `os.system()`, `os.popen()`
- `open(user_path)` without `os.path.realpath()` + prefix check

**Safe alternatives:**
- `subprocess.run([cmd, arg1, arg2])` (list form, no shell=True)
- `ast.literal_eval()` instead of `eval()`
- `parameterized queries` for DB access

## JavaScript / TypeScript (Node.js)
**Dangerous calls to flag:**
- `eval()`, `new Function(userInput)`
- `child_process.exec(userInput)` — use `.execFile()` or `.spawn()` with args array
- `innerHTML`, `document.write()`, `dangerouslySetInnerHTML` with user content
- `fs.readFile(userPath)` without path normalization
- `require(userInput)` — dynamic requires

**Safe alternatives:**
- `textContent` instead of `innerHTML`
- DOMPurify for sanitizing HTML before insertion
- `path.resolve()` + prefix check for file paths

## Java
**Dangerous patterns:**
- `Runtime.getRuntime().exec(userInput)`
- JDBC: `Statement.execute("SELECT ... " + userInput)` — use `PreparedStatement`
- `ObjectInputStream.readObject()` with untrusted data
- `ProcessBuilder` with user-controlled args spliced as single string

## Go
**Dangerous patterns:**
- `os/exec.Command("sh", "-c", userInput)`
- `fmt.Sprintf` constructing SQL strings
- `html/template` misuse — ensure `template.HTML()` not used on user content
- Goroutine access to shared maps without `sync.Mutex`

## SQL (any language)
**Always flag:**
- String concatenation or interpolation in query construction
- Dynamic `ORDER BY` column names from user input (not parameterizable — use allowlist)
- `EXECUTE` / `sp_executesql` with user input in stored procedures

## Common Secret Patterns to Flag
```
API_KEY = "sk-..."
password = "hardcoded123"
AWS_SECRET = "..."
private_key = "-----BEGIN RSA PRIVATE KEY-----"
token = "ghp_..."  # GitHub PAT
```
Even in test files, comments, or migration scripts — secrets should never be hardcoded.

## LLM-Specific (2024–2025)
If the code calls an LLM API with user-supplied content:
- Is user input inserted directly into the system prompt? (prompt injection risk)
- Is the LLM's output used to construct SQL/shell commands? (indirect injection)
- Are tool-use / function-call schemas validated before execution?
- Is there a content moderation layer before passing user text to the model?
