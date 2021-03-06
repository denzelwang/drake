# -*- python -*-
# This file contains rules for Bazel; see drake/doc/bazel.rst.

load("//tools:cpplint.bzl", "cpplint")
load(
    "//tools:drake.bzl",
    "drake_cc_binary",
    "drake_cc_googletest",
    "drake_cc_library",
)
load("//tools:gurobi.bzl", "gurobi_test_tags")
load("//tools:mosek.bzl", "mosek_test_tags")

package(default_visibility = ["//visibility:public"])

drake_cc_library(
    name = "binding",
    srcs = [],
    hdrs = ["binding.h"],
    deps = [
        ":decision_variable",
    ],
)

drake_cc_library(
    name = "constraint",
    srcs = ["constraint.cc"],
    hdrs = ["constraint.h"],
    deps = [
        "//drake/common",
        "//drake/common:autodiff",
        "//drake/common:polynomial",
        "//drake/math:matrix_util",
    ],
)

# TODO(eric.cousineau): Remove CostShim that produces coupling
drake_cc_library(
    name = "cost",
    srcs = ["cost.cc"],
    hdrs = ["cost.h"],
    deps = [
        ":constraint",
        ":function",
    ],
)

drake_cc_library(
    name = "symbolic_extraction",
    srcs = ["symbolic_extraction.cc"],
    hdrs = ["symbolic_extraction.h"],
    deps = [
        ":decision_variable",
        "//drake/common",
        "//drake/common:autodiff",
        "//drake/common:monomial",
        "//drake/common:number_traits",
        "//drake/common:polynomial",
        "//drake/common:symbolic",
    ],
)

drake_cc_library(
    name = "create_constraint",
    srcs = ["create_constraint.cc"],
    hdrs = ["create_constraint.h"],
    deps = [
        ":binding",
        ":constraint",
        ":symbolic_extraction",
        "//drake/common",
        "//drake/common:monomial",
        "//drake/common:polynomial",
        "//drake/common:symbolic",
    ],
)

drake_cc_library(
    name = "create_cost",
    srcs = ["create_cost.cc"],
    hdrs = ["create_cost.h"],
    deps = [
        ":binding",
        ":cost",
        ":symbolic_extraction",
        "//drake/common",
        "//drake/common:monomial",
        "//drake/common:polynomial",
        "//drake/common:symbolic",
        "//drake/common:unused",
    ],
)

drake_cc_library(
    name = "decision_variable",
    srcs = ["decision_variable.cc"],
    hdrs = ["decision_variable.h"],
    deps = [
        "//drake/common:symbolic",
    ],
)

drake_cc_library(
    name = "function",
    srcs = [],
    hdrs = ["function.h"],
    deps = [
        "//drake/common",
    ],
)

drake_cc_library(
    name = "solver_type",
    hdrs = ["solver_type.h"],
)

drake_cc_library(
    name = "solver_id",
    srcs = ["solver_id.cc"],
    hdrs = ["solver_id.h"],
    deps = [
        "//drake/common",
        "//drake/common:reinit_after_move",
    ],
)

# Supported solvers:
# - equality_constrained_qp_solver
# - linear_system_solver
# - moby_lcp_solver
# - IPOPT
# - MOSEK
# - NLOPT
# - SNOPT
# - Gurobi (on Ubuntu only)

# Unsupported solvers:
# - dReal

# TODO(jwnimmer-tri, david-german-tri): Support more solvers.
drake_cc_library(
    name = "mathematical_program",
    srcs = [
        "mathematical_program.cc",
    ],
    hdrs = [],
    deps = [
        ":binding",
        ":create_constraint",
        ":create_cost",
        ":decision_variable",
        ":dreal_solver",
        ":equality_constrained_qp_solver",
        ":gurobi_solver",
        ":ipopt_solver",
        ":linear_system_solver",
        ":mathematical_program_api",
        ":moby_lcp_solver",
        ":mosek_solver",
        ":nlopt_solver",
        ":snopt_solver",
        ":solver_type",
        ":symbolic_extraction",
        "//drake/common",
        "//drake/common:autodiff",
        "//drake/common:monomial",
        "//drake/common:number_traits",
        "//drake/common:polynomial",
        "//drake/common:symbolic",
    ],
)

drake_cc_library(
    name = "system_identification",
    srcs = ["system_identification.cc"],
    hdrs = ["system_identification.h"],
    deps = [
        ":mathematical_program",
        "//drake/common",
        "//drake/common:autodiff",
        "//drake/common:polynomial",
    ],
)

drake_cc_library(
    name = "rotation_constraint",
    srcs = [
        "rotation_constraint.cc",
    ],
    hdrs = [
        "rotation_constraint.h",
        "rotation_constraint_internal.h",
    ],
    deps = [
        ":mathematical_program",
        "//drake/math:gradient",
    ],
)

drake_cc_library(
    name = "rotation_constraint_visualization",
    testonly = 1,
    srcs = [
        "test/rotation_constraint_visualization.cc",
    ],
    hdrs = [
        "test/rotation_constraint_visualization.h",
    ],
    deps = [
        ":rotation_constraint",
        "//drake/common:call_matlab",
    ],
)

drake_cc_library(
    name = "add_solver_util",
    testonly = 1,
    srcs = ["test/add_solver_util.cc"],
    hdrs = ["test/add_solver_util.h"],
    deps = [
        ":mathematical_program",
    ],
)

drake_cc_library(
    name = "generic_trivial_cost",
    testonly = 1,
    srcs = [],
    hdrs = ["test/generic_trivial_costs.h"],
    deps = [
        ":constraint",
        ":cost",
    ],
)

drake_cc_library(
    name = "mathematical_program_test_util",
    testonly = 1,
    srcs = ["test/mathematical_program_test_util.cc"],
    hdrs = ["test/mathematical_program_test_util.h"],
    deps = [
        ":mathematical_program",
        "@gtest//:main",
    ],
)

drake_cc_library(
    name = "optimization_examples",
    testonly = 1,
    srcs = ["test/optimization_examples.cc"],
    hdrs = ["test/optimization_examples.h"],
    deps = [
        ":mathematical_program",
        ":mathematical_program_test_util",
        "//drake/common:eigen_matrix_compare",
        "@gtest//:main",
    ],
)

drake_cc_library(
    name = "linear_program_examples",
    testonly = 1,
    srcs = ["test/linear_program_examples.cc"],
    hdrs = ["test/linear_program_examples.h"],
    deps = [
        ":optimization_examples",
    ],
)

drake_cc_library(
    name = "quadratic_program_examples",
    testonly = 1,
    srcs = ["test/quadratic_program_examples.cc"],
    hdrs = ["test/quadratic_program_examples.h"],
    deps = [
        ":optimization_examples",
    ],
)

# The mathematical_program_api target exists so that solver targets can access
# the mathematical_program.h header, and the :mathematical_program target can
# depend on solver targets, without creating a circular dependency.
#
# TODO(hongkai-dai): MathematicalProgram should be refactored so that
# implementations of MathematicalProgramSolverInterface are not co-dependent
# with MathematicalProgram itself, and then this target should be deleted.
drake_cc_library(
    name = "mathematical_program_api",
    srcs = [
        "mathematical_program_api.cc",
        "mathematical_program_solver_interface.cc",
    ],
    hdrs = [
        "mathematical_program.h",
        "mathematical_program_solver_interface.h",
    ],
    deps = [
        ":binding",
        ":create_constraint",
        ":create_cost",
        ":decision_variable",
        ":function",
        ":solver_type",
        "//drake/common",
        "//drake/common:autodiff",
        "//drake/common:number_traits",
        "//drake/common:polynomial",
    ],
)

# Internal Solvers.

drake_cc_library(
    name = "equality_constrained_qp_solver",
    srcs = ["equality_constrained_qp_solver.cc"],
    hdrs = ["equality_constrained_qp_solver.h"],
    deps = [
        ":mathematical_program_api",
        "//drake/common",
    ],
)

drake_cc_library(
    name = "linear_system_solver",
    srcs = ["linear_system_solver.cc"],
    hdrs = ["linear_system_solver.h"],
    deps = [
        ":mathematical_program_api",
        "//drake/common",
    ],
)

drake_cc_library(
    name = "moby_lcp_solver",
    srcs = ["moby_lcp_solver.cc"],
    hdrs = ["moby_lcp_solver.h"],
    deps = [
        ":mathematical_program_api",
        "//drake/common",
    ],
)


drake_cc_library(
    name = "dreal_solver",
    srcs = ["no_dreal.cc"],
    hdrs = ["dreal_solver.h"],
    deps = [
        ":mathematical_program_api",
        "//drake/common",
    ],
)

# External Solvers.

# If --config gurobi is used, this target builds object code that satisfies
# MathematicalProgramSolverInterface by calling Gurobi. Otherwise, this target
# builds dummy object code that fails at runtime.
drake_cc_library(
    name = "gurobi_solver",
    srcs = select({
        "//tools:with_gurobi": ["gurobi_solver.cc"],
        "//conditions:default": ["no_gurobi.cc"],
    }),
    hdrs = ["gurobi_solver.h"],
    deps = select({
        "//tools:with_gurobi": [
            ":mathematical_program_api",
            "//drake/math:eigen_sparse_triplet",
            "@gurobi",
        ],
        "//conditions:default": [
            ":mathematical_program_api",
        ],
    }),
)

# If --config mosek is used, this target builds object code that satisfies
# MathematicalProgramSolverInterface by calling MOSEK. Otherwise, this target
# builds dummy object code that fails at runtime.
drake_cc_library(
    name = "mosek_solver",
    srcs = select({
        "//tools:with_mosek": ["mosek_solver.cc"],
        "//conditions:default": ["no_mosek.cc"],
    }),
    hdrs = ["mosek_solver.h"],
    deps = select({
        "//tools:with_mosek": [
            ":mathematical_program_api",
            "//drake/common:scoped_singleton",
            "@mosek",
        ],
        "//conditions:default": [
            ":mathematical_program_api",
        ],
    }),
)

# If --config snopt is used, this target builds object code that satisfies
# MathematicalProgramSolverInterface by calling SNOPT. Otherwise, this target
# builds dummy object code that fails at runtime.
drake_cc_library(
    name = "snopt_solver",
    srcs = select({
        "//tools:with_snopt": ["snopt_solver.cc"],
        "//conditions:default": ["no_snopt.cc"],
    }),
    hdrs = ["snopt_solver.h"],
    # Some versions of SNOPT's API require passing a `std::string::c_str()`
    # result of `const char*` to a function taking a `char*`, or passing a
    # function pointer that receives parameters to a function that takes a
    # (mismatched) function pointer with no arguments.  We could carefully
    # craft our snopt_solver.cc to have appropriate casts depending on the
    # signatures of the version of SNOPT in use, but choose instead to just
    # suppress all such warnings until we find some case where they matter.
    # (The next best step would be to write a sensible C++ SNOPT API wrapper,
    # and contain the language abuses there, so that the actual logic of our
    # snopt_wrapper.cc could still have full `-pedantic` checks enabled.)
    copts = ["-fpermissive"],
    deps = select({
        "//tools:with_snopt": [
            ":mathematical_program_api",
            "//drake/math:autodiff",
            "@snopt//:snopt_c",
        ],
        "//conditions:default": [
            ":mathematical_program_api",
        ],
    }),
)

drake_cc_library(
    name = "ipopt_solver",
    srcs = select({
        "//tools:no_ipopt": ["no_ipopt.cc"],
        "//conditions:default": ["ipopt_solver.cc"],
    }),
    hdrs = ["ipopt_solver.h"],
    deps = select({
        "//conditions:default": [
            "@ipopt",
            ":mathematical_program_api",
            "//drake/common:unused",
            "//drake/math:autodiff",
        ],
        "//tools:no_ipopt": [
            ":mathematical_program_api",
        ],
    }),
)

drake_cc_library(
    name = "nlopt_solver",
    srcs = select({
        "//tools:no_nlopt": ["no_nlopt.cc"],
        "//conditions:default": ["nlopt_solver.cc"],
    }),
    hdrs = ["nlopt_solver.h"],
    deps = select({
        "//conditions:default": [
            ":mathematical_program_api",
            "//drake/math:autodiff",
            "@nlopt",
        ],
        "//tools:no_nlopt": [
            ":mathematical_program_api",
        ],
    }),
)

# This library is empty unless Gurobi is available.
drake_cc_library(
    name = "gurobi_qp",
    srcs = select({
        "//tools:with_gurobi": [
            "qp.cc",
        ],
        "//conditions:default": [],
    }),
    hdrs = select({
        "//tools:with_gurobi": [
            "fast_qp.h",
            "gurobi_qp.h",
        ],
        "//conditions:default": [],
    }),
    deps = select({
        "//tools:with_gurobi": [
            "@eigen",
            "@gurobi",
        ],
        "//conditions:default": [],
    }),
)

# === test/ ===
drake_cc_googletest(
    name = "binding_test",
    srcs = [
        "test/binding_test.cc",
    ],
    deps = [
        ":binding",
        ":constraint",
        "//drake/common:symbolic_test_util",
    ],
)

drake_cc_googletest(
    name = "constraint_test",
    deps = [
        ":mathematical_program",
        "//drake/common:eigen_matrix_compare",
        "//drake/math:gradient",
    ],
)

# TODO(eric.cousineau): Remove dependence on create_cost
drake_cc_googletest(
    name = "cost_test",
    deps = [
        ":cost",
        ":create_cost",
        ":generic_trivial_cost",
        "//drake/common:eigen_matrix_compare",
        "//drake/common:is_dynamic_castable",
        "//drake/math:gradient",
    ],
)

drake_cc_googletest(
    name = "symbolic_extraction_test",
    deps = [
        ":symbolic_extraction",
        "//drake/common:eigen_matrix_compare",
        "//drake/common:symbolic_test_util",
    ],
)

# TODO(hongkai-dai): Separate this test into a test that uses Gurobi, and a
# test that doesn't.
drake_cc_googletest(
    name = "convex_optimization_test",
    srcs = [
        "test/convex_optimization_test.cc",
    ],
    # This test uses Gurobi if present, but does not require it.
    tags = gurobi_test_tags(gurobi_required = False),
    deps = [
        ":add_solver_util",
        ":mathematical_program",
        ":mathematical_program_test_util",
        "//drake/common:eigen_matrix_compare",
    ],
)

drake_cc_googletest(
    name = "decision_variable_test",
    deps = [
        ":mathematical_program",
        "//drake/common:symbolic_test_util",
    ],
)

drake_cc_googletest(
    name = "equality_constrained_qp_solver_test",
    srcs = [
        "test/equality_constrained_qp_solver_test.cc",
    ],
    deps = [
        ":mathematical_program",
        ":mathematical_program_test_util",
        "//drake/common:eigen_matrix_compare",
    ],
)

drake_cc_googletest(
    name = "gurobi_solver_test",
    srcs = [
        "test/gurobi_solver_test.cc",
    ],
    tags = gurobi_test_tags(),
    deps = [
        ":gurobi_solver",
        ":linear_program_examples",
        ":mathematical_program",
        ":mathematical_program_test_util",
        ":quadratic_program_examples",
        "//drake/common:eigen_matrix_compare",
    ],
)

drake_cc_googletest(
    name = "ipopt_solver_test",
    srcs = [
        "test/ipopt_solver_test.cc",
    ],
    deps = [
        ":linear_program_examples",
        ":mathematical_program",
        ":mathematical_program_test_util",
        ":optimization_examples",
        ":quadratic_program_examples",
        "//drake/common:eigen_matrix_compare",
    ],
)

drake_cc_googletest(
    name = "linear_complementary_problem_test",
    srcs = [
        "test/linear_complementary_problem_test.cc",
    ],
    deps = [
        ":mathematical_program",
        "//drake/common:eigen_matrix_compare",
    ],
)

drake_cc_googletest(
    name = "linear_system_solver_test",
    srcs = [
        "test/linear_system_solver_test.cc",
    ],
    deps = [
        ":mathematical_program",
        ":mathematical_program_test_util",
        ":optimization_examples",
        "//drake/common:eigen_matrix_compare",
    ],
)

drake_cc_googletest(
    name = "mathematical_program_test",
    srcs = [
        "test/mathematical_program_test.cc",
    ],
    deps = [
        ":generic_trivial_cost",
        ":mathematical_program",
        ":mathematical_program_test_util",
        "//drake/common:eigen_matrix_compare",
        "//drake/common:is_dynamic_castable",
        "//drake/common:symbolic_test_util",
    ],
)

# TODO(hongkai-dai): Separate this test into a test that uses Gurobi, and a
# test that doesn't.
drake_cc_googletest(
    name = "mixed_integer_optimization_test",
    srcs = [
        "test/mixed_integer_optimization_test.cc",
    ],
    # This test uses Gurobi if present, but does not require it.
    tags = gurobi_test_tags(),
    deps = [
        ":add_solver_util",
        ":mathematical_program",
        ":mathematical_program_test_util",
        "//drake/common:eigen_matrix_compare",
    ],
)

drake_cc_googletest(
    name = "moby_lcp_solver_test",
    deps = [
        ":mathematical_program",
        "//drake/common:eigen_matrix_compare",
    ],
)

drake_cc_googletest(
    name = "mosek_solver_test",
    srcs = [
        "test/mosek_solver_test.cc",
    ],
    tags = mosek_test_tags(),
    use_default_main = False,
    deps = [
        ":linear_program_examples",
        ":mathematical_program",
        ":mathematical_program_test_util",
        ":quadratic_program_examples",
    ],
)

drake_cc_googletest(
    name = "nonlinear_program_test",
    srcs = [
        "test/nonlinear_program_test.cc",
    ],
    deps = [
        ":mathematical_program",
        ":mathematical_program_test_util",
        ":optimization_examples",
        "//drake/common:eigen_matrix_compare",
        "//drake/common:is_dynamic_castable",
    ],
)

drake_cc_googletest(
    name = "rotation_constraint_test",
    size = "medium",
    # Excluding because an assertion fails in LLVM code. Issue #6179.
    tags = gurobi_test_tags() + mosek_test_tags() + ["no_tsan"],
    deps = [
        ":gurobi_solver",
        ":mathematical_program",
        ":rotation_constraint",
        "//drake/common:eigen_matrix_compare",
        "//drake/math:geometric_transform",
    ],
)

drake_cc_binary(
    name = "plot_feasible_rotation_matrices",
    testonly = 1,
    srcs = ["test/plot_feasible_rotation_matrices.cc"],
    deps = [
        ":mathematical_program",
        ":rotation_constraint",
        "//drake/common:call_matlab",
    ],
)

drake_cc_googletest(
    name = "rotation_constraint_limit_test",
    size = "large",
    # Excluding because an assertion fails in LLVM code. Issue #6179.
    tags = gurobi_test_tags() + ["no_tsan"],
    deps = [
        ":gurobi_solver",
        ":mathematical_program",
        ":rotation_constraint",
        "//drake/common:eigen_matrix_compare",
    ],
)

drake_cc_googletest(
    name = "snopt_solver_test",
    srcs = [
        "test/snopt_solver_test.cc",
    ],
    deps = [
        ":linear_program_examples",
        ":mathematical_program",
        ":mathematical_program_test_util",
        ":optimization_examples",
        ":quadratic_program_examples",
        "//drake/common:eigen_matrix_compare",
    ],
)

drake_cc_googletest(
    name = "solver_id_test",
    deps = [
        ":solver_id",
    ],
)

drake_cc_googletest(
    name = "system_identification_test",
    # The test takes ~20 sec in opt mode but ~140 sec in dbg mode.
    # To avoid timeouts in dbg and valgrind, we need "large" here.
    size = "large",
    deps = [
        ":system_identification",
        "//drake/common:eigen_matrix_compare",
    ],
)

drake_cc_binary(
    name = "plot_rotation_mccormick_envelope",
    testonly = 1,
    srcs = ["test/plot_rotation_mccormick_envelope.cc"],
    deps = [
        ":rotation_constraint_visualization",
    ],
)

# The extra_srcs are required here because cpplint() doesn't understand how to
# extract labels from select() functions yet.
cpplint(extra_srcs = [
    "fast_qp.h",
    "gurobi_qp.h",
    "gurobi_solver.cc",
    "gurobi_solver.h",
    "ipopt_solver.cc",
    "ipopt_solver.h",
    "mosek_solver.cc",
    "mosek_solver.h",
    "nlopt_solver.cc",
    "nlopt_solver.h",
    "no_gurobi.cc",
    "no_ipopt.cc",
    "no_mosek.cc",
    "no_nlopt.cc",
    "no_snopt.cc",
    "qp.cc",
    "snopt_solver.cc",
    "snopt_solver.h",
])
