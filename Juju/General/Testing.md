Add this to pyproject.toml to make unit tests work:
```
[tool.pytest.ini_options] 
minversion = "6.0" 
log_cli_level = "INFO" 
# Add below
pythonpath = [ 
	"lib", 
	"src" 
]
```

integration test for debugging:
```
tox -e integration -- --tb long -sl --model=machine --keep-models -k lifecycle
```