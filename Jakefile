var OS = require("os"),
    ENV = require("system").env,
    FILE = require("file"),
    JAKE = require("jake"),
    task = JAKE.task,
    CLEAN = require("jake/clean").CLEAN,
    FileList = JAKE.FileList,
    framework = require("cappuccino/jake").framework,
    browserEnvironment =
require("objective-j/jake/environment").Browser,
    configuration = ENV["CONFIG"] || ENV["CONFIGURATION"] || ENV["c"] ||
"Debug";

framework ("UIKit", function(task)
{   
    task.setBuildIntermediatesPath(FILE.join(ENV["CAPP_BUILD"],
"UIKit.build", configuration));
    task.setBuildPath(FILE.join(ENV["CAPP_BUILD"], configuration));

    task.setProductName("UIKit");
    task.setIdentifier("org.cappuccino.UIKit");
    task.setVersion("0.9");
    task.setAuthor("Amari Robinson");
    task.setSummary("Mobile application development framework.");
    task.setSources(new FileList("*.j"));
    task.setResources(new FileList("Resources/**/*"));
    task.setInfoPlistPath("Info.plist");

    if (configuration === "Debug")
        task.setCompilerFlags("-DDEBUG -g");
    else
        task.setCompilerFlags("-O");
});

task ("debug", function()
{
    ENV["CONFIGURATION"] = "Debug";
    JAKE.subjake(["."], "build", ENV);
});

task ("release", function()
{
    ENV["CONFIGURATION"] = "Release";
    JAKE.subjake(["."], "build", ENV);
});

task ("default", ["release"]);

task ("build", ["UIKit"]);

task ("install", ["debug", "release"])

task ("symlink-narwhal", ["release", "debug"], function()
{
    var frameworksPath = require("tusk").getPackagesDirectory();
    ["Release", "Debug"].forEach(function(aConfig)
    {
        print("Symlinking " + aConfig + " ...");

        if (aConfig === "Debug")
            frameworksPath = FILE.join(frameworksPath, aConfig);

        var buildPath = FILE.absolute(FILE.join(ENV["CAPP_BUILD"], aConfig, "UIKit")),
            symlinkPath = FILE.join(frameworksPath, "UIKit");

        OS.system(["sudo", "ln", "-s", buildPath, symlinkPath]);
    });
});