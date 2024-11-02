# Kubito Helm Charts PR Checklist

Thanks for contributing! Please check the following to make sure your PR is ready:

- [ ] **Chart Version**: Have you updated the `version` in `Chart.yaml` in the relevant chart directory?
- [ ] **README Update**: If you've changed `values.yaml`, run the `readme-generator-for-helm` tool to update the `README.md` file:

  ```bash
  readme-generator -v charts/your-chart/values.yaml -r charts/your-chart/README.md
  ```

- [ ] **Description**: Briefly describe your changes and why they are necessary.

- [ ] **Testing**: Have you tested the chart locally to ensure your changes work as expected?

---

## Description of Changes

> Provide a summary of your changes here. Explain why this PR is necessary and what it adds to the repository.

---

## Related Issues

> Link any related issues here, for example:
>
> - Fixes #123

---

## Additional Notes

> Add any other information that might help reviewers understand this PR better.
