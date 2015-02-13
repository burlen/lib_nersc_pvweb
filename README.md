# lib_nersc_pvweb
A JavaScript library for web portals hosted at NERSC which make use of ParaView running on HPC systems.

## lib_nersc_pvweb.js
Core job management code. The primary function of interest is
```javascript
function launch_job(job_params)
```
Calling this will launch a job and start the job monitor.
### Event handlers
```javascript
function on_session_created(session_md)
function on_job_status(session_md, stat_code)
function on_job_ready(session_md)
function on_job_canceled(session_md)
function on_job_queued(session_md)
function on_job_monitor_error(session_md)
function on_load_log(session_md, log_str)
```
Set some or all of the event handlers to adapt your UI and respond to job state changes.  For example, on_job_ready is called when one should start the ParaView web app. Event handlers should be installed by calling the set\_&lt;event handlker name&gt; method.

### Site configuration
```javascript
function set_web_host(fqdn)
function set_paraview_prefix(sys_name, path)
function set_paraview_version(ver)
```
Before launching jobs be sure to set the site specific parameters, such as your site's fully qualified domain name, path and version for ParaView installs you will use on one or more HPC reosurces.

## lib_nersc_pvweb_ui.js
Uses lib_nersc_pvweb.js and generates a simple job management UI and launcher for ParaView web apps on NERSC systems. The following snippet shows how to use it.
```javascript
<!-- NERSC ParaView Web -->
<script src="lib/lib_nersc_pvweb/lib_nersc_pvweb.js"></script>
<script src="lib/lib_nersc_pvweb/lib_nersc_pvweb_ui.js"></script>
<script language="Javascript">
    $(document).ready( function() {
        // disable logging
        var console = {};
        console.log = function(){};
        window.console = console;
        // configure core
        lib_nersc_pvweb.set_web_host('portal-auth.nersc.gov')
        lib_nersc_pvweb.set_paraview_version('4.2.0-PDACS')
        lib_nersc_pvweb.set_paraview_prefix('/usr/common/graphics/ParaView')
        // configure ui
        lib_nersc_pvweb_ui.initialize()
        lib_nersc_pvweb_ui.set_web_app('apps/pvweb_visualizer.html')
        lib_nersc_pvweb_ui.create_submit_form($('#form_area'))
        lib_nersc_pvweb_ui.create_job_table($('#job_table'))
    });
</script>
```

## Install
The libraries are used by sourcing them into your html. They depend on NEWT, JQuery, and ParaView web.
