# Ecosystem versioning

This is a proposal to solve [versioning issues in the p6c ecosystem](https://github.com/Raku/problem-solving/issues/72).

## Proposed solution

Right now, [distributing modules](https://docs.raku.org/language/modules#Distributing_modules) is but a section in the tutorial that explains modules. This means it's not as well indexed as it should, and the documentation on META6.json should probably be revised and improved. This proposal involves creating an exclusive "Releasing your module", spinning that section off `modules`, in the following steps.

1. Revise thoroughly all steps involved, from the definition of the different keys of the META6.json file, to other underdocumentaed parts like how to use a build phase. I have created [this issue](https://github.com/Raku/doc/issues/3439) to address that previous work.
2. Spin off to a `create-module` file, that includes that revised version as well as documenting how to have several versions released to the ecosystem at the same time either hosted at CPAN or at the ecosystem. That would pick up [suggestions made here](https://github.com/Raku/problem-solving/issues/72). A new issue in the doc repo will be created to do this.
