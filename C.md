def calculate_column_capacity(b, D, fck, fy, bar_diameter, num_bars):
    """
    Calculates ultimate axial load capacity of a short column.
    b: Width of column (mm)
    D: Depth/Thickness of column (mm)
    fck: Characteristic strength of concrete (N/mm^2)
    fy: Yield strength of steel (N/mm^2)
    """
    import math

    # 1. Total cross-sectional area of column
    gross_area = b * D
    
    # 2. Area of longitudinal steel reinforcement (Asc)
    single_bar_area = (math.pi / 4) * (bar_diameter ** 2)
    steel_area = single_bar_area * num_bars
    
    # 3. Net area of concrete (Ac)
    concrete_area = gross_area - steel_area
    
    # 4. Steel reinforcement ratio check (Standard limit: 0.8% to 6%)
    steel_ratio = (steel_area / gross_area) * 100
    
    # 5. Limit state load computation (Standard design code formula)
    # Pu = 0.4 * fck * Ac + 0.67 * fy * Asc
    pu_newtons = (0.4 * fck * concrete_area) + (0.67 * fy * steel_area)
    pu_kilo_newtons = pu_newtons / 1000
    
    # Display Results
    print(f"--- Column Analysis Results ---")
    print(f"Gross Area: {gross_area:.0f} mm² | Steel Area: {steel_area:.0f} mm²")
    print(f"Steel Percentage: {steel_ratio:.2f}%")
    
    if steel_ratio < 0.8:
        print("⚠️ Warning: Steel ratio is below the standard minimum limit (0.8%).")
    elif steel_ratio > 6.0:
        print("⚠️ Warning: Steel ratio exceeds the practical maximum limit (6.0%).")
        
    return pu_kilo_newtons

# Example Execution
column_width = 300       # mm
column_depth = 400       # mm
concrete_grade = 25      # M25 concrete (25 N/mm^2)
steel_grade = 415        # Fe415 steel (415 N/mm^2)
rebar_size = 16          # 16mm diameter bars
rebar_count = 6          # 6 longitudinal bars

ultimate_load = calculate_column_capacity(column_width, column_depth, concrete_grade, steel_grade, rebar_size, rebar_count)
print(f"Ultimate Axial Load Capacity (Pu): {ultimate_load:.2f} kN")
