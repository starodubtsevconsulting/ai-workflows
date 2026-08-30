# Workflow role capability matrix template

The adjacent [role-capability-matrix.csv](role-capability-matrix.csv) and
[role-capability-ownership.csv](role-capability-ownership.csv) are empty reusable schemas. They grant no capability.

Each agent-enabled workflow copies both schemas into its workflow-local agent directory. It fills the role matrix with
the exact initialized roles and extends the ownership header with those role IDs before adding capability rows. Every
role contract links its readable top capability declaration to those two filled workflow-local matrices.

The readable declaration summarizes the effective boundary; the filled CSV files are the mechanical routing authority.
A missing link, empty workflow-local matrix, undeclared role, or declaration/matrix disagreement fails closed.
