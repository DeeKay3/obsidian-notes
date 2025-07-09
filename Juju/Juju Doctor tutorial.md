Create k8s model:
`juju add model workshop`

Install juju doctor (python 3.12.3 works):
`pip install juju doctor`
or
`uvx juju-doctor --help`

Clone the repo for testing:
`git clone --branch temp/workshop-solution git@github.com:canonical/juju-doctor.git`

Using juju-doctor, executing probes:
`juju-doctor check --probe "file://tests/resources/probes/python/failing.py" --probe "file://tests/resources/probes/python/passing.py" --status tests/resources/artifacts/status.yaml --model workshop`

- Deploy Alertmanager and Prometheus [bundle](https://github.com/canonical/juju-doctor/blob/temp/workshop-solution/tests/resources/artifacts/workshop/bundle.yaml)
- Relate alertmanager and prometheus
- create a probe which has assertions for [all artifacts](https://github.com/canonical/juju-doctor/tree/temp/workshop-solution/tests/resources/artifacts/workshop) (status, bundle, show-unit):
	- Status ⇾ assert Alertmanager is active/idle
	- Bundle ⇾ assert that Alertmanager’s config_file is set, otherwise the default is used
	- Show-unit ⇾ assert a relation to Prometheus exists, over the alerting endpoint
```py
am = "alertmanager-k8s"
prom = "prometheus-k8s"


def status(juju_statuses):
    for status in juju_statuses.values():
        assert (
            status["applications"][am]["units"][f"{am}/0"]["juju-status"]["current"] == "idle"
        ), f"{am} is not in `idle` status."


def bundle(juju_bundles):
    for bundle in juju_bundles.values():
        assert "options" in bundle["applications"][am], f"There are no configs set for {am}."
        assert "config_file" in bundle["applications"][am]["options"], (
            f"{am} `config_file` option not set. The charm is using the default config."
        )


def show_unit(juju_show_units):
    for show_unit in juju_show_units.values():
        assert any(
            rel["endpoint"] == "alerting" and f"{prom}/0" in rel["related-units"]
            for rel in show_unit[f"{am}/0"]["relation-info"]
        ), f"{am} and {prom} are not related over the `alerting` endpoint"
```

Solution and workshop files: https://github.com/canonical/juju-doctor/blob/temp/workshop-solution/tests/resources/probes/python/workshop/solution.py