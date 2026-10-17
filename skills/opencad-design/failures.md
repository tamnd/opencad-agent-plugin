# Failures and recoveries

Each row gives the failure, the usual cause, the recovery and the most retries before you ask the user. After the last retry, write in DESIGN.md what you tried and what failed, then ask the user. Do not try the same change two times.

| Failure | Usual cause | Recovery | Retries |
| --- | --- | --- | --- |
| Fillet fails | The radius is larger than a face next to the edge, or the chain of edges crosses a seam | The message gives the largest radius that builds. Use a radius below it, fillet before the feature that made the short face, or split the chain | 2 |
| Chamfer fails | The size is larger than a face next to the edge | Make the size smaller than the shortest face next to the edge | 2 |
| Shell fails | The wall is thicker than the smallest concave radius | Shell before the small fillets. Make the wall thinner. Report the face to the user | 2 |
| Boolean is not manifold | The faces of the tool and the body are on the same plane or touch | Move the tool 0.01 mm, or extend it through the body. Never accept a result that is not manifold | 2 |
| Loft or sweep twists | The profiles have a different count of vertices, or the path has a kink | Give each section the same count of vertices. Make the path smooth | 2 |
| Reference is ambiguous | A split made two faces from one | Use a ranked candidate from the diagnostic, or ref_resolve with `where` | 1 |
| Reference matched by signature | A sketch was drawn again | Confirm the face with ref_resolve. Name it again by the sketch entity that makes it | 1 |
| Sketch has degrees of freedom left | A dimension is missing | Add the dimension that the stage `dof` names | 2 |
| Interference | A real clash, or a fit that you did not declare | Measure the overlap with `clearance`. If the fit is intended, declare it. If not, move the part in the skeleton | 2 |
| Part is mirrored or flipped | The direction of a frame is wrong | Look at the frames with model_describe. Fix the frame in the skeleton, not in the part | 1 |
| Wrong units | A number with no unit where a length goes (`OCL0102`) | Add the unit, as in `6 mm` | 1 |
| Unknown function (`OCL0202`) | The function is not in the stdlib | Call docs_search. Never invent a function | 1 |
| Unknown parameter (`OCL0110`) or a missing one (`OCL0111`) | The call does not match the signature | Call docs_search for the function and use its parameter names | 1 |
| Build is slow and gives a job | The model is large | Call job_get with `wait_s`. Do not start the same build again | 1 |
| `kernel_crash` or `timeout` | OCCT failed on this geometry | Build the features one at a time to find the one that fails. Change it, or ask the user | 1 |

A `requires` that fails is not a failure of the tool. The model works, and the design does not meet the requirement. Change the geometry. Never change the limit.
