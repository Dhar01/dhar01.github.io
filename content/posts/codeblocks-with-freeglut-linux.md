---

title: "Setting up Codeblocks with Freeglut on Linux"
date: 2021-02-06T01:34:53+06:00
tags: ["how to"]
draft: false

---

In my Computer Graphics class, the teacher asked us to setup Codeblocks for the glut. She gave us a tutorial on how to setup Codeblocks on windows but didn't provide any suggestion to set up on Linux. After some searching on google, I was able to setup Codeblocks with **Freeglut** (*which is an alternative to Glut and available on Linux*). This is a kind of writeup (*or tutorial*) on how I was able to setup Codeblocks with Freeglut.

> This setup is tested and working on Pop os, as of writing, should be working with Ubuntu/Debian with the latest Codeblocks.

Also, I linked to the posts from where I collect these and setup Codeblocks to work properly.

# Installation Part

To work with all of these, of course, we have to install all the necessary files.

For Freeglut:

```
sudo apt install g++ freeglut3 freeglut3-dev

sudo apt install libxmu-dev libxi-dev
```

For Codeblocks:

```
sudo apt install codeblocks
```

For [OpenGL](https://en.wikibooks.org/wiki/OpenGL_Programming/Installation/Linux):

```
sudo apt install build-essential libgl1-mesa-dev

sudo apt install libglew-dev libsdl2-dev libsdl2-image-dev libglm-dev libfreetype6-dev  # some libraries
```

Check OpenGL installation:

```
glxinfo | grep OpenGL
```

# Setup Part

Open Codeblocks, go to `settings > Global Variables`.
Click `new`, it should look like this:

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/mbr16y8x8ejw824ka5ox.png)

type `freeglut`, press ok and setup the rest like this:

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/1hez3gb1srizxze6bqbl.png)

Now close the Codeblocks.

Resting part is collected from [here](http://forums.codeblocks.org/index.php?topic=17291.0).

- Go to `/usr/share/codeblocks/templates`, create the file `freeglut.cbp` and put the following code there:

```
<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
<CodeBlocks_project_file>
    <FileVersion major="1" minor="4" />
    <Project>
        <Option title="freeglut" />
        <Option pch_mode="0" />
        <Option compiler="gcc" />
        <Build>
            <Target title="default">
                <Option output="freeglut.exe" />
                <Option type="0" />
                <Option compiler="gcc" />
                <Option includeInTargetAll="1" />
            </Target>
        </Build>
        <Compiler>
            <Add directory="$(#freeglut.include)" />
        </Compiler>
        <Linker>
            <Add library="freeglut" />
            <Add library="glu32" />
            <Add library="opengl32" />
            <Add library="winmm" />
            <Add library="gdi32" />
            <Add library="user32" />
            <Add library="kernel32" />
            <Add directory="$(#freeglut.lib)" />
        </Linker>
        <Unit filename="main.cpp">
            <Option compilerVar="CPP" />
            <Option target="default" />
        </Unit>
    </Project>
</CodeBlocks_project_file>
```

Save it.

- Create a directory named **freeglut** on `/usr/share/codeblocks/templates/wizard/`. You will find another directory named **glut** there. Copy all the contents and paste it on the newly created **freeglut** directory.

- Now go to the created **freeglut** directory, edit the `wizard.script` file and paste the following:

```
////////////////////////////////////////////////////////////////////////////////
//
// FreeGLUT project wizard
//
////////////////////////////////////////////////////////////////////////////////

// globals
FreeGlutPathDefault    <- _T("$(#freeglut)");
FreeGlutPathDefaultInc <- _T("$(#freeglut.include)");
FreeGlutPathDefaultLib <- _T("$(#freeglut.lib)");
FreeGlutPath <- _T("");

function BeginWizard()
{
    local intro_msg = _T("Welcome to the new FreeGLUT project wizard!\n\n" +
                         "This wizard will guide you to create a new project\n" +
                         "using the FreeGLUT OpenGL extensions.\n\n" +
                         "When you 're ready to proceed, please click \"Next\"...");

    local glutpath_descr = _T("Please select the location of FreeGLUT on your computer.\n" +
                              "This is the top-level folder where FreeGLUT was installed (unpacked).\n" +
                              "To help you, this folder must contain the subfolders\n" +
                              "\"include\" and \"lib\".");

    Wizard.AddInfoPage(_T("GlutIntro"), intro_msg);
    Wizard.AddProjectPathPage();
    if (PLATFORM == PLATFORM_MAC)
    {
        FreeGlutPathDefault="/System/Library/Frameworks/FreeGLUT.framework";
    }
    else
        Wizard.AddGenericSelectPathPage(_T("FreeGlutPath"), glutpath_descr, _T("Please select FreeGLUT's location:"), FreeGlutPathDefault);
    Wizard.AddCompilerPage(_T(""), _T("*"), true, true);
}

////////////////////////////////////////////////////////////////////////////////
// GLUT's path page
////////////////////////////////////////////////////////////////////////////////

function OnLeave_GlutPath(fwd)
{
    if (fwd)
    {
        local dir         = Wizard.GetTextControlValue(_T("txtFolder")); // txtFolder is the text control in GenericSelectPathPage
        local dir_nomacro = VerifyDirectory(dir);

        if (dir_nomacro.IsEmpty())
            return false;

        // verify include dependencies
        local dir_nomacro_inc = GetCompilerIncludeDir(dir, FreeGlutPathDefault, FreeGlutPathDefaultInc);
        if (dir_nomacro_inc.IsEmpty())
            return false;
        if (!VerifyFile(dir_nomacro_inc + wxFILE_SEP_PATH + _T("GL"), _T("freeglut.h"), _T("FreeGLUT's include"))) return false;

        // verify library dependencies
        local dir_nomacro_lib = GetCompilerLibDir(dir, FreeGlutPathDefault, FreeGlutPathDefaultLib);
        if (dir_nomacro_lib.IsEmpty())
            return false;

        if (PLATFORM == PLATFORM_MSW)
        {
            if (!VerifyLibFile(dir_nomacro_lib, _T("freeglut"), _T("FreeGLUT's"))) return false;
        }
        else
        {
            if (!VerifyLibFile(dir_nomacro_lib, _T("freeglut"), _T("FreeGLUT's"))) return false;
        }


        FreeGlutPath = dir; // Remember the original selection.

        local is_macro = _T("");

        // try to resolve the include directory as macro
        is_macro = GetCompilerIncludeMacro(dir, FreeGlutPathDefault, FreeGlutPathDefaultInc);
        if (is_macro.IsEmpty())
        {
            // not possible -> use the real inc path we had computed instead
            FreeGlutPathDefaultInc = dir_nomacro_inc;
        }

        // try to resolve the library directory as macro
        is_macro = GetCompilerLibMacro(dir, FreeGlutPathDefault, FreeGlutPathDefaultLib);
        if (is_macro.IsEmpty())
        {
            // not possible -> use the real lib path we had computed instead
            FreeGlutPathDefaultLib = dir_nomacro_lib;
        }
    }
    return true;
}

// return the files this project contains
function GetFilesDir()
{
    return _T("glut/files");
}

// setup the already created project
function SetupProject(project)
{
    // set project options
    if (PLATFORM != PLATFORM_MAC)
    {
        project.AddIncludeDir(FreeGlutPathDefaultInc);
        project.AddLibDir(FreeGlutPathDefaultLib);
    }

    // add link libraries
    if (PLATFORM == PLATFORM_MSW)
    {
        project.AddLinkLib(_T("freeglut"));
        project.AddLinkLib(_T("opengl32"));
        project.AddLinkLib(_T("glu32"));
        project.AddLinkLib(_T("winmm"));
        project.AddLinkLib(_T("gdi32"));
    }
    else if (PLATFORM == PLATFORM_MAC)
    {
        project.AddLinkerOption(_T("-framework GLUT"));
        project.AddLinkerOption(_T("-framework OpenGL"));

        project.AddLinkerOption(_T("-framework Cocoa")); // GLUT dependency
    }
    else
    {
        project.AddLinkLib(_T("glut"));
        project.AddLinkLib(_T("GL"));
        project.AddLinkLib(_T("GLU"));
        project.AddLinkLib(_T("Xxf86vm"));
    }

    // enable compiler warnings (project-wide)
    WarningsOn(project, Wizard.GetCompilerID());

    // Debug
    local target = project.GetBuildTarget(Wizard.GetDebugName());
    if (!IsNull(target))
    {
        target.SetTargetType(ttConsoleOnly); // ttConsoleOnly: console for debugging
        target.SetOutputFilename(Wizard.GetDebugOutputDir() + Wizard.GetProjectName() + DOT_EXT_EXECUTABLE);
        target.SetWorkingDir(FreeGlutPath + _T("/bin"));
        // enable generation of debugging symbols for target
        DebugSymbolsOn(target, Wizard.GetCompilerID());
    }

    // Release
    target = project.GetBuildTarget(Wizard.GetReleaseName());
    if (!IsNull(target))
    {
        target.SetTargetType(ttExecutable); // ttExecutable: no console
        target.SetOutputFilename(Wizard.GetReleaseOutputDir() + Wizard.GetProjectName() + DOT_EXT_EXECUTABLE);
        target.SetWorkingDir(FreeGlutPath + _T("/bin"));
        // enable optimizations for target
        OptimizationsOn(target, Wizard.GetCompilerID());
    }

    return true;
}
```

Save it.

- Now on `/usr/share/codeblocks/templates/wizard` directory, there is a script named `config.script` and add the following line at the end of the file:

```
RegisterWizard(wizProject,     _T("freeglut"),     _T("FreeGLUT project"),      _T("2D/3D Graphics"));
```

- Now go to this [link](http://freeglut.sourceforge.net/index.php#download), this is the Freeglut project's source files, download the latest stable:

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/fjj0vpb4it4h7m019qff.png)

- Extract it.
- Copy the contents of the extracted folder and paste it on the `/usr/share/codeblocks/templates/wizard/freeglut` directory. At the time of writing, it looks like this:

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/u4a6p1e0vtvdp1w2nf8x.png)

# Codeblocks Setup Part

If everything goes well/correctly, Codeblocks should be open without warning/showing any errors.

- Open Codeblocks. Go to `File > New > Projects` and this dialog should be showing with **Freeglut** Project:

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/y2pa24rn1u9tm72uqup5.png)

- Give a name of the project as you see fit:

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/psjcznzq2vlrvzjtyzxi.png)

- Then, specify the location where the Freeglut is:

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/fenew0dfdnd90hqurosi.png)

- Leave the next dialog as it is (*don't change anything*):

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/8t821ivowtp62b2u03e9.png)

At this moment your Codeblocks should be ready for the Freeglut projects.

# Final Part

Just checking if things are working or not.

- Go to sources, there should be a `main.cpp` file for test running:

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/87yljltqn34dxjsahem4.png)

- Build and Run!

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/i46ookx1e592pxvirdo8.png)

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/pxpufqfqax0g9kicouog.png)

That's it.

Okay, that's all. Hope, everything works fine.
See ya later!
