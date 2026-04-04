# Why does pg_cron process run with whatever username is stored under cron.schedule?

Because that is how the execution security context is preserved.

`pg_cron` runs the schedule with prviliges of whatever user have scheduled a particular job.

When a job is scheduled, the information about current user is stored under `cron.job.username`, and when the job executes, it opens a Postgres connection or starts a worker using that metadata.

That prevents scheduling and running a job as a different user, eg. superuser, and avoids privileges confusion by scoping each job to a user creating it.
