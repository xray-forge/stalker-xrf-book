# Smart terrains and jobs

Smart terrain configs describe places where squads and individual objects can work. Job configs decide which object can
take which logic section and with what priority.

The source files live mostly under `src/engine/configs/scripts/**/smart` plus shared smart terrain configs such as
`sim_smart_base.ltx` and `sim_smart_resource.ltx`.

## Smart terrain role

A smart terrain is a server-side simulation object. It owns jobs, tracks assigned objects, and participates in offline
ALife simulation. When an assigned object goes online, its binder and scheme logic run on the client side.

Use smart terrain configs for simulation placement and job selection. Use scheme sections for the actual object behavior
once the job is active.

## Job fields

Common job fields include:

- `logic`, pointing to the script config and section used by the object;
- `prior`, defining selection priority;
- `suitable`, checking whether the object can take the job;
- `active`, selecting the active logic section;
- `path_walk` and `path_look`, used by walker-style schemes;
- `on_info` and other condlists for switching job state.

The exact fields depend on the scheme used by the job.

## Editing workflow

When changing a smart terrain job:

1. find the smart terrain section and job section;
2. follow `logic` or `active` to the scheme section;
3. check `suitable` and `prior` if the NPC does not select the job;
4. check scheme implementation and parser support for any new field;
5. validate LTX includes and schema.

Run:

```powershell
npm run cli verify ltx
```
