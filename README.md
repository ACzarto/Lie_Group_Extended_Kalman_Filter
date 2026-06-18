# Lie_Group_Extended_Kalman_Filter
This project extends the traditional Euclidean Extended Kalman Filter (EKF) to the Lie group framework to address estimation problems involving states evolving on SO(3), resulting in the Lie Group Extended Kalman Filter (LG-EKF).
$$
\mathbf{p}_{n+1} = \mathbf{p}_n + \mathbf{v}_n \Delta T + \frac{1}{2}\left[\mathbf{R}_n(\mathbf{a}_{m,n} - \mathbf{a}_{b,n} - \mathbf{\epsilon}_{a,n}) + \mathbf{g}\right](\Delta T)^2
$$

$$
\mathbf{v}_{n+1} = \mathbf{v}_n + \left[\mathbf{R}_n(\mathbf{a}_{m,n} - \mathbf{a}_{b,n} - \mathbf{\epsilon}_{a,n}) + \mathbf{g}\right]\Delta T
$$

$$
\mathbf{R}_{n+1} = \mathbf{R}_n \exp\left(\left[(\mathbf{\omega}_{m,n} - \mathbf{\omega}_{b,n} - \mathbf{\epsilon}_{\omega,n})\Delta T\right]^\wedge \right)
$$

$$
\mathbf{a}_{b,n+1} = \xi_a \mathbf{a}_{b,n} + \mathbf{\epsilon}_{b,n}^{a}
$$

$$
\mathbf{\omega}_{b,n+1} = \xi_{\omega}\mathbf{\omega}_{b,n} + \mathbf{\epsilon}_{b,n}^{\omega}
$$
