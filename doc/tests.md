# Tests
Some test tools like Behat, PHP-unit are in place, but make sure to run the following command before running them:
```bash
task APP_ENV=test db:reset
```

## Unit test & coverage

```bash
task test:unit

## Code coverage -> ./report folder
task test:report
```

## PHP-Stan, PHP-CS

```bash
## PHP stan
task qa:stan

## PHP CS
task qa:cs
```