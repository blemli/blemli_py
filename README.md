# blemli
More convenience

## usage

### from and to date

```python
import blemli
blemli.from_date(now())
>>> "2025-01-31"
blemli.to_date("2025-01-31")
>>> (2025,1,31)
```



## ideas

### Get credentials

```python
def get_credentials(uid):
    try:
        cmd = ["op", "item", "get", uid, "--format", "json"]
        result = subprocess.run(cmd, capture_output=True, text=True)
        if result.returncode != 0:
            raise Exception(f"Failed to get credentials from 1Password: {result.stderr}")    
        data = json.loads(result.stdout)
        username = None
        password = None
        for field in data.get("fields", []):
            if field.get("label") == "username":
                username = field.get("value")
            elif field.get("label") == "password":
                password = field.get("value")
        if not username or not password:
            raise Exception("Username or password not found in 1Password item")
        return username, password
    except Exception as e:
        click.echo(f"Error retrieving credentials: {str(e)}", err=True)
        sys.exit(1)
```

